---
Description: Entwerfen Sie Ihre App so, dass sie gut aussieht und gut auf Fernsehgeräten funktioniert.
title: Entwerfen für Xbox und Fernsehgeräte
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
---
<!--Note to Eliot from Linda: see my comments by searching on v-lcap; after your review, I recommend removing all commented out text unless you think you may need it later; it just gets in the way of an already long doc-->

> \[In diesem Artikel wird ein Feature beschrieben, das noch nicht verfügbar ist. Dieses Feature unterliegt vor der kommerziellen Freigabe möglicherweise grundlegenden Änderungen. Microsoft übernimmt keine Garantie, weder ausdrücklicher noch impliziter Art, für die hier bereitgestellten Informationen.\]

# Entwerfen für Xbox und Fernsehgeräte

Entwerfen Sie Ihre App für die Universelle Windows-Plattform (UWP) so, dass sie gut aussieht und gut auf Xbox One- und Fernsehbildschirmen funktioniert.

## Übersicht

Dank der universellen Windows-Plattform können Sie großartige Benutzeroberflächen für verschiedene Windows 10-Geräte erstellen. 
Die Mehrzahl der durch das UWP Framework bereitgestellten Funktionen ermöglichen Apps, auf diesen Geräten ohne zusätzlichen Aufwand die gleiche Benutzeroberfläche (UI) zu verwenden. 
Das Anpassen und Optimieren Ihrer App an Xbox One- und Fernsehbildschirme erfordert jedoch besondere Überlegungen.

Die Erfahrung, die Sie machen, wenn Sie auf dem Sofa sitzen und mittels eines Gamepads oder einer Fernbedienung mit Ihrem Fernsehgerät interagieren, wird als **3-Meter-Erfahrung** (10-Fuß-Erfahrung) bezeichnet. 
Der Name kommt daher, dass sich der Benutzer im Allgemeinen ungefähr 3 Meter (10 Fuß) vom Bildschirm entfernt befindet. 
Dies stellt eine besondere Herausforderung dar, die beispielsweise bei einer *50-cm-Erfahrung* (2-Fuß-Erfahrung) oder bei der Interaktion mit einem PC nicht vorhanden ist. 
Wenn Sie eine App für Xbox One oder ein anderes Gerät entwickeln, das Inhalte auf einem Fernsehbildschirm ausgibt und eine Steuerung verwendet, sollten Sie dies stets bedenken.

Nicht alle Schritte in diesem Artikel sind erforderlich, damit Ihre App gut in 10 Fuß-Umgebungen funktioniert. Wenn Sie diese jedoch kennen und für Ihre App die entsprechenden Entscheidungen treffen, führt dies zu einer besseren 10-Fuß-Erfahrung, die an die spezifischen Anforderungen Ihrer App angepasst ist. 
Berücksichtigen Sie die folgenden Designrichtlinien, wenn Sie eine App für eine 10-Fuß-Umgebung entwickeln.

### Einfach

Ein Entwurf für eine 10-Fuß-Umgebung bedeutet einen einzigartigen Satz von Herausforderungen. Auflösung und Anzeigeabstand kann es Menschen schwer machen, zu viele Informationen zu verarbeiten. 
Versuchen Sie, Ihr Design klar zu halten und auf die einfachsten Komponenten zu reduzieren, die möglich sind. Die Menge der auf einem Fernseher angezeigten Informationen sollte mit der vergleichbar sein, die Ihnen auf einem Mobiltelefon angezeigt werden, nicht auf einem Desktop.

![Xbox One-Startseite](images/designing-for-tv/xbox-home-screen.png)

### Einheitlich

UWP-Apps in 10-Fuß-Umgebungen sollten intuitiv und benutzerfreundlich sein. Sorgen Sie für einen klaren und unverkennbaren Fokus. 
Ordnen Sie Inhalte so an, dass Verschiebungen auf dem Bildschirm konsistent und vorhersagbar sind. Stellen Sie Menschen den kürzesten Weg zu dem bereit, was sie tun möchten.

![Xbox One-Film-App](images/designing-for-tv/xbox-movies-app.png)

_**Alle im Screenshot gezeigten Filme sind auf Microsoft Filme & TV verfügbar.**_  

### Fesselnd

Der große Bildschirm bietet äußerst faszinierende Erfahrungen, ähnlich wie ein Kino. Rand-Rand-Design, elegante Bewegungen und brillante Farben und Typografien eröffnen Ihren Apps neue Dimensionen. Seien Sie mutig, und bieten Sie ein ansprechendes Design.

![Xbox One Avatar-App](images/designing-for-tv/xbox-avatar-app.png)

### Optimierungen für die 10-Fuß-Erfahrung

Da Sie nun mit den Grundsätzen eines guten UWP-App-Designs für die 10-Fuß-Erfahrung vertraut sind, lesen Sie die folgenden Übersicht über die verschiedenen Arten, wie Sie Ihre App optimieren und eine hervorragende Benutzerumgebung bereitstellen können.
<!--[v-lcap] I recommend shortening the descriptions to the bare minimum, and focusing on the rest in the actual sections. I also rearranged the order of this table to map to the order you have in the document.-->


| Feature        | Beschreibung           |
| -------------------------------------------------------------- |--------------------------------|
| [Anpassen von Benutzeroberflächenelementen](#ui-element-sizing)  | Die universelle Windows-Plattform verwendet [Skalierung und effektive Pixel](..\layout\design-and-ui-intro.md#effective-pixels-and-scaling), um die Benutzeroberfläche gemäß dem Anzeigeabstand zu skalieren. Wenn auf Xbox One eine UWP-App ausgeführt wird, wird ein entsprechender Standardskalierungsfaktor verwendet, damit alle Benutzeroberflächenelemente vom Sofa aus gut erkennbar sind, beispielsweise Texte und allgemeine Steuerelemente. Wenn Sie verstehen, wie Sie Größen anpassen und auf Ihre Benutzeroberfläche anwenden, hilft Ihnen dies, Ihre App für die 10-Fuß-Umgebung zu optimieren.  |
|  [Fernsehsicherer Bereich](#tv-safe-area) | Nicht alle Fernsehgeräte zeigen die Inhalte von Rand zu Rand an. Dies hat historische und technische Gründe. Die UWP vermeidet automatisch und standardmäßig die Anzeige von Benutzeroberflächenelementen in unsicheren Bereichen (nahe dem Bildschirmrand). Dies führt jedoch zu einem „Schachteleffekt“, so dass die Benutzeroberfläche einem Briefkastenschlitz ähnelt. Damit Ihre App auf Fernsehgeräten wirklich immersiv ist, müssen Sie diese so anpassen, dass sie sich auf Fernsehgeräten, die dies unterstützen, bis zu den Rändern erweitert wird. |
| [Farben](#colors)  |  Die UWP unterstützt Farbdesigns. Daher wird eine App, die das Systemdesign berücksichtigt, auf Xbox One standardmäßig auf **dark** festgelegt. Wenn Ihre App ein bestimmtes Farbdesign verwendet, sollten Sie daran denken, dass sich einige Farben nicht gut für Fernsehbildschirme eignen und daher vermieden werden sollten. Um optimale Ergebnisse zu erzielen, sollten Sie für Ihre App eine fernsehgerätspezifische Farbpalette erstellen. |
| [Gamepad und Fernbedienung](#gamepad-and-remote-control)      | Der wichtigste Schritt bei der Optimierung für 10-Fuß-Umgebungen besteht darin, sicherzustellen, dass Ihre App gut mit Gamepads und Fernbedienungen funktioniert. Die korrekte Unterstützung von Tastaturinteraktionen und Navigation trägt dazu bei, dass Gamepad- und Fernbedienungseingaben vergleichsweise gut funktionieren. Es gibt jedoch spezifische Verbesserungen für Gamepads und Fernbedienungen, mit denen Sie die Interaktionserfahrungen von Benutzern auf Geräten optimieren können, auf denen ihre Aktionen eher eingeschränkt sind. |
| [Mausmodus](#mouse-mode)|In einigen Benutzeroberflächen, beispielsweise Karten und Zeichenoberflächen, ist es nicht möglich oder praktisch, eine XY-Fokusnavigation zu verwenden. Für diese Schnittstellen stellt die UWP den **Mausmodus** bereit, damit das Gamepad/die Fernbedienung ähnlich einer Maus auf einem Desktopcomputer frei navigieren kann.|
| [Fokusanzeige](#focus-visual)  | Die Fokusanzeige ist der Rahmen um das Benutzeroberflächenelement, das zurzeit den Fokus besitzt. Dies hilft Benutzern, sich zu orientieren, damit sie in Ihrer Benutzeroberfläche einfach navigieren können, ohne die Orientierung zu verlieren. Während die Fokusanzeige auf verschiedenen Windows 10-Geräten funktioniert, sollten Sie sicherstellen, dass sie leicht auf dem Fernsehbildschirm zu erkennen ist und sich standardmäßig an einer Stelle befindet, mit der Benutzer häufig interagieren. Wenn der Fokus nicht klar erkennbar ist, könnten Benutzer in der Benutzeroberfläche die Orientierung verlieren und keine gute Erfahrung mit Ihrer App machen. Darüber hinaus sollten Sie [Overlays für einfaches Ausblenden](#light-dismiss-overlay) verwenden, um die Aufmerksamkeit auf die Benutzeroberflächenkomponente zu ziehen, mit denen der Benutzer zurzeit interagiert.  |
| [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) | Die UWP stellt eine **XY Fokusnavigation** bereit, mit deren Hilfe Benutzer in der Benutzeroberfläche Ihrer App navigieren können. Dies begrenzt Benutzer jedoch auf eine Navigation nach oben, unten, links und rechts. In diesem Abschnitt finden Sie Empfehlungen für den Umgang mit diesen und anderen Überlegungen. Möglicherweise müssen Sie auch [Fokusinteraktionen](#focus-engagement) nutzen (durch Drücken der **Eingabe/Auswahl**-Schaltfläche zur Interaktion mit einem Benutzeroberflächenelement), um bestimmte Steuerelemente zu verwenden, beispielsweise Schieberegler. Hierdurch wird die Anzahl der Klicks reduziert, die benötigt werden, um von einer Seite des Bildschirms zur anderen zu gelangen. | 
| [Richtlinien für Benutzeroberflächensteuerelemente](#guidelines-for-ui-controls)  |  Es gibt bestimmte Verbesserungen, die Sie an Ihrer App vornehmen können, die für alle Windows 10-Geräte sinnvoll sind, nicht nur Xbox One- oder andere 10-Fuß-Umgebungen. Anstatt lediglich die 10-Fuß-Erfahrung zu verbessern, sollten Sie diese bewährten Methoden, die besonders für Fernsehgeräte nützlich sind, für alle von der UWP unterstützten Geräte in Betracht ziehen.  |

<!--[v-lcap] "Focus engagement" is an H2 section that precedes "XY focus"; I recommend either putting it in its own row in the table ahead of XY focus, or changing it to an H3 section under "XY focus," whichever makes more sense; just as long as the overview table  matches what you do-->

<!--| [Sound](../style/sound.md)  |  Sounds play a key role in the 10-foot experience, helping to immerse and give feedback to the user. The UWP provides functionality that automatically turns on sounds for common controls when the app is running on Xbox One. Find out more about the sound support built into the UWP and learn how to take advantage of it. |-->


<!--[v-lcap] I had to put this table into markdown because the links weren't rendering in HTML.
<table>
    <tr>
        <td> [Gamepad and remote control](#gamepad-and-remote-control)</td>
        <td>Making sure that your app works well with gamepad and remote is the most important step in optimizing for 10-foot experiences. Properly supporting keyboard interaction and navigation helps get gamepad and remote control input working relatively well. However, there are gamepad and remote-specific improvements that you can make to optimize the user interaction experience on a device where their actions are somewhat limited.
        </td>
    </tr>
    <tr>
        <td>[XY focus navigation and interaction](#xy-focus-navigation-and-interaction)</td>
        <td>The UWP provides **XY focus navigation** that allows the user to navigate around your app's UI. However, this limits the user to navigating up, down, left, and right. Recommendations for dealing with this and other considerations are outlined in this section. You may also need to utilize [focus engagement](#focus-engagement) (by pressing the **Enter/Select** button to interact with a UI element) to use certain controls, such as sliders. This cuts down on the number of clicks required to get from one side of the screen to the other.
        </td>
    </tr>
    <tr>
        <td>[Mouse mode](#mouse-mode)</td>
        <td>In some user interfaces, such as maps and drawing surfaces, it is not possible or practical to use XY focus navigation. For these interfaces, the UWP provides **mouse mode** to let the gamepad/remote navigate freely, like a mouse on a desktop computer.
        </td>
    </tr>
    <tr>
        <td>[Focus visual](#focus-visual)</td>
        <td>The focus visual is the border around the UI element that currently has focus. This helps orient the user so that they can easily navigate your UI without getting lost. While focus visual works across multiple Windows 10 devices, you will want to make sure that it is easy to see on TV and that it defaults to a location that the user will interact with often. If the focus is not clearly visible, the user could get lost in your UI and not have a great experience. Also, take advantage of [Light dismiss overlay](#light-dismiss-overlay) to call attention to UI that a user is currently interacting with.
        </td>
    </tr>
    <tr>
        <td>[UI element sizing](#ui-element-sizing)</td>
        <td>
            The Universal Windows Platform uses [scaling and effective pixels](..\layout\design-and-ui-intro.md#effective-pixels-and-scaling) to scale the UI according to the viewing distance. 
            When a UWP app is running on Xbox One, it uses an appropriate default scale factor in order to make all the UI elements, such as text and common controls, easily visible from your couch across the room. 
            Understanding sizing and applying it across your UI will help optimize your app for the 10-foot environment.
        </td>
    </tr>
    <tr>
        <td>[TV-safe area](#tv-safe-area)</td>
        <td>
            Not all TVs display content all the way to the edge of the screen due to historical as well as technological reasons. 
            The UWP will automatically avoid displaying any UI in unsafe areas (areas close to the edges of the screen) by default. However, this creates a "boxed-in" effect in which the UI looks letterboxed. 
            For your app to be truly immersive on TV, you will want to modify it so that it extends to the edges of the screen on TVs that support it.
        </td>
    </tr>
    <tr>
        <td>[Colors](#colors)</td>
        <td>
            The UWP supports color themes, and an app that respects the system theme will default to **dark** on Xbox One. 
            If your app has a specific color theme, you should consider that some colors don't work well for TV and should be avoided. 
            For the best possible results, consider creating a TV-specific color palette for your app.
        </td>
    </tr>
    <tr>
        <td>[Sound](../style/sound.md)</td>
        <td>
            Sounds play a key role in the 10-foot experience, helping to immerse and give feedback to the user. The UWP provides functionality that automatically turns on sounds for common controls when the app is running on Xbox One. Find out more about the sound support built into the UWP and learn how to take advantage of it.
        </td>
    </tr>
    <tr>
        <td>[Guidelines for UI controls](#guidelines-for-ui-controls)</td>
        <td>
            There are certain improvements you can make to your app that make sense for all Windows 10 devices, not just Xbox One or other 10-foot experiences. 
            Rather than only improving the 10-foot experience, consider applying these best practices across all devices supported by the UWP, which are particularly beneficial for TV.
        </td>
    </tr>
</table>-->

<!--### Gamepad and remote interaction

Users of your TV app won't be interacting with the UI with a high-precision input device such as a mouse, and this needs to be taken into account when designing your app's interface. Visibility of the **focus visual** (the border highlighting the UI element that the user is currently navigated to) is also key to making sure that the user doesn't get lost.

#### [Gamepad and remote control](#gamepad-and-remote-control)

Making sure that your app works well with gamepad and remote is the most important step in optimizing for TV. As mentioned in [Running UWP apps on Xbox One](#running-uwp-apps-on-xbox-one), properly supporting keyboard interaction and navigation helps get gamepad and remote control input working relatively well. However, there are gamepad- and remote-specific improvements that you can make to optimize the user interaction experience on a device where their actions are somewhat limited.

#### [Focus visual](#focus-visual)

While focus visual works across multiple Windows 10 devices, you will want to make sure that it is easy to see on TV and that it defaults to a location that the user will interact with often. If the focus is not clearly visible, the user could get lost in your UI and not have a great experience.

#### [2D focus navigation and interaction](#2d-focus-navigation-and-interaction)

UWP provides 2D navigation that allows the user to navigate around your app's UI. However, this limits the user to navigating up, down, left, and right. Recommendations for dealing with this and other considerations are outlined in [2D navigation best practices](#2d-navigation-best-practices). You may also need to utilize [focus engagement](#focus-engagement) (pressing the **Enter/Accept** button to interact with a UI element) to use certain controls, such as sliders. This cuts down on the number of clicks required to get from one side of the screen to the other.

#### [Mouse mode](#mouse-mode)

In some user interfaces, such as maps and drawing surfaces, it is not possible or practical to use 2D navigation. For these interfaces, UWP provides **mouse mode** to let the gamepad/remote navigate freely, like a mouse on a desktop computer.

### Layout and content density

Unlike with desktop apps, in which the user is close to the screen, a user interacting with your app on TV will likely be sitting in a couch across the room. Because of this, reducing content density is important so that the user can easily navigate your app. Remember: simplicity is key.

#### [UI element sizing](#ui-element-sizing)

The Universal Windows Platform uses [scaling and effective pixels](../layout/grid.md) to scale the UI according to the viewing distance. When a UWP app is running on Xbox One, it uses an appropriate default scale factor in order to make all the UI elements, such as text and common controls, easily visible from your couch across the room. Understanding sizing and applying it across your UI will help optimize your app for TV.

#### [TV-safe area](#tv-safe-area)

Not all TVs display content all the way to the edge of the screen due to historical as well as technological reasons. UWP will automatically avoid displaying any UI in unsafe areas (areas close to the edges of the screen) by default. However, this creates a "boxed-in" effect in which the UI looks letterboxed. For your app to be truly immersive on TV, you will want to modify it so that it extends to the edges of the screen on TVs that support it.

### Styles for TV

When designing for TV, there are special considerations in regards to color and sound. The styles you use may work great for other devices, but might need some optimizing to make them shine on TV.

#### [Colors](#colors)

UWP supports color themes, and an app that respects the system theme will default to **dark** on Xbox One. If your app has a specific color theme, you should consider that some colors don't work well for TV and should be avoided. For the best possible results, consider creating a TV-specific color palette for your app.

#### [Sound](../style/sound.md)

Sounds play a key role in TV apps, helping immerse the user in the interactive experience. Find out more about the sound support built into UWP and learn how to take advantage of it.

### [Improvements for all platforms](#improvements-for-all-platforms)

There are certain improvements you can make to your app that make sense for all Windows 10 platforms, not just Xbox One or other TV experiences. Rather than only improving the TV experience, consider applying these best practices that apply across all platforms UWP supports, and are particularly beneficial for TV.
-->

<!--## Running UWP apps on Xbox One

Some features for UWP in the 10-foot environment are built-in, and you will get these without any additional work once you properly port to Xbox One. However, there are other optimizations you may want to consider making so that your app performs as well as it can on Xbox. Learn about these features in the following sections.

### Gamepad and remote control support

Arrow key and tab stop behavior (pressing **Tab** to get to the next UI element) on keyboard informs how 2D navigation moves focus through the **D-pad** on a game controller or remote. The **Enter** and **Space** keys are also automatically mapped to the **Enter/Accept** key (see [Gamepad and remote control](#gamepad-and-remote-control)).

The quality of gamepad and remote behavior that you get out of the box depends on how well keyboard is supported in your app, and thus may vary greatly from app to app. A good way to ensure that your app will work well with gamepad/remote is to make sure that it works well with keyboard on PC, and then test with gamepad/remote to find weak spots in your UI.

### TV-safe area

Using the [VisibleBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx) functionality of UWP (for more information, see [TV-safe area](#tv-safe-area)), the following will be done automatically for your app:

* All UI elements are drawn inside the TV-safe area
* The page's background colors are drawn all the way to the edges of the TV

![TV-safe area](images/designing-for-tv/tv-safe-area.png)

### UI scaling

UWP supports proper scaling of UI based on the settings of the system on which the app is currently running. On desktop, this setting can be found in **Settings > System > Display** as a sliding value. This same setting exists on phone as well if the device supports it.

![Change the size of text, apps, and other items](images/designing-for-tv/ui-scaling.png)

On Xbox One there is no such system setting, however in order for UWP UI elements to be sized appropriately for TV, they are scaled at a default of **200%**. As long as UI elements are appropriately sized for other platforms, they will be appropriately sized for TV as well. For more information, see [UI element sizing](#ui-element-sizing).


### Focus visual

The same focus visual can be used for keyboard as well as game controller input, and will always be highly visible on any platform that supports focus visual, including Xbox One and the 10-foot experience. For more information, see [Focus visual](#focus-visual).

### Sound

UWP provides functionality that automatically turns on sounds for common controls when the app is running on Xbox One. This helps provide feedback to the user that they have interacted with the app in some way. For more information, see [Sound](../style/sound.md).

### Accent color and high contrast colors

As long as your app uses a brush resource such as **SystemControlForegroundAccentBrush**, or a color resource (**SystemAccentColor**), or instead calls accent colors directly through the [UIColorType.Accent*](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) API, those colors are replaced with accent colors appropriate for TV. High contrast brush colors are also pulled in from the system the same way as on PC and phone, but with TV-appropriate colors.

### Light dismiss overlay

In order to call the user's attention to the UI elements that the user is currently manipulating with the game controller or remote control, UWP automatically adds a "smoke" layer that covers areas outside of the popup UI when the app is running on Xbox One. This requires no extra work, but is something to keep in mind when designing your UI.
-->

## Anpassen von Benutzeroberflächenelementen

Da Benutzer von Apps in 10-Fuß-Umgebungen Fernbedienungen oder Gamepads verwenden und sich mehrere Meter vom Bildschirm entfernt befinden, gibt es einige Überlegungen in Bezug auf die Benutzeroberfläche, die Sie für Ihren Entwurf berücksichtigen müssen. 
Stellen Sie sicher, dass die Benutzeroberfläche über eine geeignete Inhaltsdichte verfügt und nicht zu überladen ist, damit Benutzer einfach navigieren und Elemente auswählen können. Denken Sie daran: Einfachheit ist der Schlüssel.

### Skalierungsfaktor und adaptives Layout

Der **Skalierungsfaktor** hilft, sicherzustellen, dass Benutzeroberflächenelemente in der richtigen Größe für das Gerät angezeigt werden, auf dem die App ausgeführt wird. 
Auf Desktops befindet sich diese Einstellung in **Einstellungen > System > Anzeige** als gleitender Wert. 
Diese Einstellung ist auch auf Mobiltelefonen vorhanden, wenn das Gerät diese unterstützt.

![Ändern der Größe von Text, Apps und anderen Elementen](images/designing-for-tv/ui-scaling.png) 

Auf Xbox One ist diese Systemeinstellung nicht vorhanden. UWP-Benutzeroberflächenelemente werden jedoch standardmäßig auf **200 %** skaliert, um ihre Größe an Fernsehbildschirme anzupassen. 
Solange Benutzeroberflächenelemente für andere Geräte korrekt angepasst werden, werden sie auch für Fernsehgeräte angepasst. 
Xbox One rendert Ihre App mit 1080p (1920 x 1080 Pixel). Stellen Sie daher beim Portieren einer App von anderen Geräten, beispielsweise PCs, 
sicher, dass die Benutzeroberfläche bei 960 x 540 px und einer Skalierung auf 100 % gut aussieht [adaptive Techniken](https://msdn.microsoft.com/en-us/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design).

Das Entwerfen für Xbox unterscheidet sich etwas vom Entwerfen für PCs, da Sie lediglich eine Auflösung von 1920 x 1080 berücksichtigen müssen. 
Es spielt keine Rolle, ob der Benutzer ein Fernsehgerät mit einer besseren Auflösung besitzt. UWP-Apps werden stets auf 1080p skaliert.

Bei der Ausführung auf Xbox One werden darüber hinaus korrekte Ressourcengrößen aus dem Satz mit 200 % für Ihre App aufgerufen, unabhängig von der Auflösung des Fernsehgeräts.

### Inhaltsdichte

Denken Sie beim Entwerfen Ihrer App daran, dass Benutzer die Benutzeroberfläche aus der Entfernung sieht und mittels einer Fernbedienung oder einem Gamecontroller mit dieser interagiert. Dies dauert länger als die Verwendung von Maus- oder Toucheingaben.

#### Größen von Benutzeroberflächensteuerelementen

Interaktive Benutzeroberflächenelemente sollten eine Mindesthöhe von 32 epx (effektive Pixel) besitzen. Dies ist die Standardeinstellung für allgemeine UWP-Steuerelemente. Wenn diese bei einer Skalierung von 200 % verwendet wird, ist sichergestellt, dass Benutzeroberflächenelemente aus der Entfernung erkennbar sind. Darüber hinaus trägt dies zur Reduzierung der Inhaltsdichte bei. 

<!--For more information about effective pixels, see [Effective pixels](../layout/grid.md#effective-pixels).-->

![UWP-Schaltfläche bei einer Skalierung von 100 % und 200 %](images/designing-for-tv/button-100-200.png)

#### Anzahl der Klicks

Um Ihre Benutzeroberfläche zu vereinfachen, sollten Benutzer nicht mehr als **sechs Klicks** benötigen, wenn sie von einem Rand des Fernsehbildschirms zum anderen navigieren. Auch hier gilt der Grundsatz der **Einfachheit**. Weitere Informationen finden Sie unter [Pfad der wenigsten Klicks](#path-of-least-clicks).

![6 Symbole insgesamt](images/designing-for-tv/six-clicks.png)

### Textgrößen

Wenden Sie die folgenden Faustregeln an, damit Ihre Benutzeroberfläche aus der Entfernung erkennbar ist:

* Haupttext und Lesen von Inhalten: mindestens 15 epx
* Nicht kritische Texte und ergänzende Inhalte: mindestens 12 epx

Wenn Sie in der Benutzeroberfläche einen größeren Text verwenden, sollten Sie eine Größe wählen, die die verfügbare Bildschirmfläche nicht zu sehr begrenzt, indem sie Platz beansprucht, der potenziell von anderen Inhalten ausgefüllt werden kann.

### Deaktivieren des Skalierungsfaktors

Wir empfehlen Ihnen, für Ihre App die Vorteile der Skalierungsfaktorunterstützung zu nutzen. Dies trägt dazu bei, dass sie auf allen Geräten korrekt ausgeführt wird, indem sie für jeden Gerätetyp skaliert wird. 
Es ist jedoch möglich, dieses Verhalten zu deaktivieren und alle Benutzeroberflächenelemente für eine Skalierung von 100 % zu entwerfen. Beachten Sie, dass Sie den Skalierungsfaktor nicht in einen anderen Wert als 100 % ändern können.

Sie können den Skalierungsfaktor deaktivieren, indem Sie den folgenden Codeausschnitt verwenden:

```csharp
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` informiert Sie, ob die Deaktivierung erfolgreich ausgeführt wurde.

Stellen Sie sicher, dass Sie korrekten Größen von Benutzeroberflächenelementen berechnen, indem Sie die in diesem Thema genannten *effektiven* Pixelwerte auf die *tatsächlichen* Pixelwerte verdoppeln.

## Fernsehsicherer Bereich

Nicht alle Fernsehgeräte zeigen die Inhalte von Rand zu Rand an. Dies hat historische und technische Gründe. Standardmäßig vermeidet die UWP die Anzeige von Benutzeroberflächeninhalten in nicht fernsehsicheren Bereichen und zeichnet stattdessen lediglich den Seitenhintergrund.

Der nicht fernsehsichere Bereich wird in der folgenden Abbildung durch den blauen Bereich dargestellt.

![Nicht fernsehsicherer Bereich](images/designing-for-tv/tv-unsafe-area.png)

Sie können für den Hintergrund eine statische Farbe, eine Designfarbe oder ein Bild verwenden, wie in den folgenden Codeausschnitten gezeigt.

### Designfarbe

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### Bild

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

So sieht Ihre App ohne zusätzlichen Aufwand aus.

![Fernsehsicherer Bereich](images/designing-for-tv/tv-safe-area.png)

Dies ist nicht optimal, da dies der App einen „Schachteleffekt“ verleiht. Teile der Benutzeroberfläche werden scheinbar abgeschnitten, beispielsweise der Navigationsbereich und das Raster. Sie können jedoch Optimierungen durchführen, um Teile der Benutzeroberfläche bis an die Ränder des Bildschirms zu erweitern, um der App einen kinofilmähnlicheren Effekt zu verleihen.

### Zeichnen der Benutzeroberfläche bis zum Rand

Wir empfehlen Ihnen, bestimmte Benutzeroberflächenelemente zu verwenden, um die Benutzeroberfläche bis an die Ränder des Bildschirms zu erweitern und Benutzern eine immersivere Umgebung zu bieten. Dazu gehören [ScrollViewers](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx), [Navigationsbereiche](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane) und [CommandBars](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx).

Es ist jedoch auch wichtig, dass interaktive Elemente und Texte stets die Bildschirmränder vermeiden, um sicherzustellen, dass sie auf bestimmten Fernsehgeräten nicht abgeschnitten werden. Wir empfehlen Ihnen, nur nicht essentielle visuelle Elemente bis zu 5 % von den Rändern des Bildschirms entfernt zu zeichnen. Wie in [Anpassen von Benutzeroberflächenelementen](#ui-element-sizing) bereits erwähnt, nutzt eine UWP-App, die den Xbox One-Standardskalierungsfaktor von 200 % verwendet, einen Bereich von 960 x 540 epx. Sie sollten es daher vermeiden, in der Benutzeroberfläche Ihrer App essentielle Benutzerflächenelemente in den folgenden Bereichen zu platzieren:

- 27 epx vom oberen und unteren Rand
- 48 epx vom linken und rechten Rand

Es gibt zwei Möglichkeiten, die Benutzeroberfläche bis zu den Rändern des Bildschirms zu erweitern: *Kernfenstergrenzen* und *negative Ränder*.

### Kernfenstergrenzen

Im Fall von UWP-Apps, die ausschließlich für die 10-Fuß-Erfahrung entwickelt werden, stellt die Verwendung von Kernfenstergrenzen die einfachere Option dar.

Fügen Sie in der `OnLaunched`-Methode von `App.xaml.cs` den folgenden Code hinzu:

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

Mit dieser Codezeile wird das App-Fenster bis zu den Rändern des Bildschirms erweitert. Daher müssen Sie alle interaktiven und essentiellen Benutzeroberflächenelemente in den bereits beschriebenen fernsehsicheren Bereich verschieben. Kurzlebige Benutzeroberflächenelemente wie Kontextmenüs und geöffnete [Kombinationsfelder](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.combobox.aspx) bleiben automatisch im fernsehsicheren Bereich.

![Kernfenstergrenzen](images/designing-for-tv/core-window-bounds.png)

### Negative Ränder

Im Fall von UWP-Apps, die für eine Vielzahl von Geräten entwickelt werden, beispielsweise Mobiltelefone, Desktops und Xbox One, stellen negative Ränder möglicherweise die intuitivere Methode für die Anpassung adaptiver Layouts dar. 
Wir empfehlen Ihnen, einen [benutzerdefinierten Auslöser](#custom-visual-state-trigger-for-xbox-one) zu erstellen und die Ränder für Fernsehlayouts zu ändern.

#### Bereichshintergründe 

<!--[v-lcap] Do you mean "panel" or "pane" in this title and paragraph? 03-29-16 - updated to "pane" per Chigusa-->

Navigationsbereiche werden in der Regel nahe am Rand des Bildschirms gezeichnet. Daher sollte sich der Hintergrund in den nicht fernsehsicheren Bereich erstrecken, um schlecht aussehende Lücken zu vermeiden. 
Sie erreichen dies, indem Sie negative Ränder für das [SplitView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.splitview.aspx)-Steuerelement verwenden, 
das häufig als Komponente für Navigationsbereiche verwendet wird, und positive Ränder für die Inhalte von `SplitView` verwenden, um sicherzustellen, dass diese innerhalb des fernsehsicheren Bereichs angezeigt werden.

![Navigationsbereich bis an die Ränder des Bildschirms erweitert](images/designing-for-tv/tv-safe-areas-2.png)

Hier wurde der Hintergrund des Navigationsbereichs bis an die Ränder des Bildschirms erweitert, während die Navigationselemente weiterhin im fernsehsicheren Bereich angezeigt werden. 
Der Inhalte von `SplitView` (in diesem Fall ein Raster mit Elementen) wurde bis an den unteren Rand des Bildschirms erweitert, damit es aussieht, als würde es fortgesetzt und nicht abgeschnitten. Der obere Bereich des Rasters befindet sich nach wie vor im fernsehsicheren Bereich. Später in diesem Abschnitt wird gezeigt, wie Sie sicherstellen, dass das fokussierte Element weiterhin im fernsehsicheren Bereich angezeigt wird.

Mit dem folgenden Codeausschnitt wird dieser Effekt erzielt:

```xml
<SplitView x:Name="RootSplitView"
           Margin="-48, -27">
            <SplitView.Pane>
                 <ListView x:Name="NavMenuList"
                           Margin="0,75,0,27"
                           ContainerContentChanging=
                                "NavMenuItemContainerContentChanging"
                           ItemContainerStyle="{StaticResource 
                                NavMenuItemContainerStyle}"
                           ItemTemplate="{StaticResource NavMenuItemTemplate}"
                           ItemInvoked="NavMenuList_ItemInvoked"/>
            </SplitView.Pane>
            <Frame x:Name="frame"
                   Margin="0,27,48,27"
                   Navigating="OnNavigatingToPage"
                   Navigated="OnNavigatedToPage"/>
    </SplitView>
```

[CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) ist ein weiteres Beispiel für einen Bereich, der häufig in der Nähe eines oder mehrerer Ränder der App positioniert ist. Daher sollte dessen Hintergrund auf Fernsehbildschirmen bis an die Ränder des Bildschirms erweitert werden. In der Regel enthält sie am rechten Ende die Schaltfläche **Mehr**, dargestellt durch „...“, die weiter im fernsehsicheren Bereich angezeigt werden sollte. Im Folgenden finden Sie einige unterschiedliche Strategien, um die gewünschten Interaktionen und visuellen Effekte zu erzielen.

**Option 1**: Ändern der `CommandBar`-Hintergrundfarbe in transparent oder in die Farbe des Seitenhintergrunds:

```xml
<CommandBar x:Name="topbar" 
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

Hierdurch sieht die `CommandBar` aus, als ob sie auf dem gleichen Hintergrund wie der Rest der Seite angezeigt wird, sodass sich der Hintergrund nahtlos bis an den Rand des Bildschirms erstreckt.

**Option 2**: Fügen Sie ein Hintergrundrechteck hinzu, dessen Füllung die gleiche Farbe wie der `CommandBar`-Hintergrund hat, und erweitern Sie dieses bis an die Ränder des Bildschirms mit negativen Rändern:

```xml
<Rectangle VerticalAlignment="Top" 
            HorizontalAlignment="Stretch" 
            Margin="0,-27,-48,0"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar" 
            VerticalAlignment="Top" 
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> **Hinweis**& Nbsp; & Nbsp;Wenn Sie sich für diesen Ansatz entscheiden, sollten Sie berücksichtigen, dass die Schaltfläche **Mehr** die Höhe der geöffneten `CommandBar` ändert, wenn notwendig, um die Beschriftungen der `AppBarButton`-Schaltflächen unterhalb der Symbole anzuzeigen. Wir empfehlen Ihnen, die Beschriftungen *rechts* neben ihren Symbolen anzuzeigen, um diese Größenanpassung zu vermeiden.

#### Hintergrundbilder und Medienelemente

Die meisten Bilder und anderen Medienelemente enthalten keine kritischen Informationen an den Rändern. Daher ist es sicher, diese Benutzeroberflächenelemente bis an die Ränder des Bildschirms zu zeichnen, um eine immersive Erfahrung zu bieten. Der folgende Codeausschnitt zeigt ein Beispiel dafür, wie ein Bild bis zu den Rändern des Bildschirms gezeichnet wird:

```xml
<Image Source="\Assets\HeaderBackground.png" 
       Stretch="Uniform" 
       Height="227" 
       VerticalAlignment="Top" 
       Margin="-48,-27,-48,0"/>
```

Sie können dies natürlich auch für Medien durchführen, beispielsweise Videos.

#### Bildlaufenden von Listen und Rastern

Es ist für Listen und Raster üblich, mehr Elemente zu enthalten als gleichzeitig auf den Bildschirm passen. Wenn dies der Fall ist, sollten Sie die Liste oder das Raster bis zum Rand des Bildschirms erweitern. Listen und Raster mit horizontalem Bildlauf sollten bis zum rechten Rand erweitert werden. Listen und Raster mit vertikalem Bildlauf sollten bis zum unteren Rand erweitert werden.

![Abschneiden eines Rasters im fernsehsicheren Bereich](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

Wenn eine Liste oder ein Raster wie hier beschrieben erweitert wird, ist es wichtig, dass die Fokusanzeige und das mit dieser verknüpfte Element weiter im fernsehsicheren Bereich angezeigt werden.

![Bildlauffokus sollte weiter im fernsehsicheren Bereich angezeigt werden](images/designing-for-tv/scrolling-grid-focus.png)

Die UWP verfügt über Funktionen, die dafür sorgen, dass die Fokusanzeige weiter innerhalb der [VisibleBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx) angezeigt wird. Sie müssen jedoch Abstand hinzufügen, um sicherzustellen, dass für die Listen-/Rasterelemente ein Bildlauf in den Anzeigebereich des sicheren Bereichs durchgeführt werden kann. Genauer gesagt, müssen Sie für [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx) oder [GridView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) deren [ItemsPresenter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx) einen positiven Rand hinzufügen, wie im folgenden Codeausschnitt gezeigt:

```xml
<Style x:Key="TitleSafeListViewStyle" 
       TargetType="ListView">
    <Setter Property="Margin" 
            Value="0,0,0,-27"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="ListView">
                    <Border BorderBrush="{TemplateBinding BorderBrush}" 
                            Background="{TemplateBinding Background}" 
                            BorderThickness="{TemplateBinding BorderThickness}">
                        <ScrollViewer x:Name="ScrollViewer"
                                      TabNavigation="{TemplateBinding TabNavigation}"
                                      HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                      HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                      IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                      VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                      VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                      IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                      IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                      IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                      ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                      IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                      BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                      AutomationProperties.AccessibilityView="Raw">
                            <ItemsPresenter Header="{TemplateBinding Header}"
                                            HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                            HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                            Footer="{TemplateBinding Footer}"
                                            FooterTemplate="{TemplateBinding FooterTemplate}"
                                            FooterTransitions="{TemplateBinding FooterTransitions}"
                                            Padding="{TemplateBinding Padding}" 
                                            Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

Sie platzieren den zuvor angezeigten Codeausschnitt entweder in die Seitenressourcen oder in die app-Ressourcen und greifen anschließend wie folgt auf diesen zu:

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> **Hinweis**&nbsp;&nbsp;Dieser Codeausschnitt gilt speziell für `ListView`-Elemente. Im Fall von `GridView`-Elementen legen Sie das [TargetType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.controltemplate.targettype.aspx)-Attribut für [ControlTemplate](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.controltemplate.aspx) und [Style](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.style.aspx) auf `GridView` fest.


### Benutzerdefinierter visueller Zustandsauslöser für Xbox One <a name="custom-visual-state-trigger-for-xbox-one"></a>

Um Ihre UWP-App an die 10-Fuß-Erfahrung anzupassen, empfehlen wir Ihnen, das Layout zu ändern, wenn die App erkennt, dass sie auf einer Xbox-Konsole gestartet wurde. Sie tun dies, indem Sie einen benutzerdefinierten visuellen Zustandsauslöser verwenden, wie im folgenden Codeausschnitt gezeigt:

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.Margin" 
                        Value="-48,-27"/>
                <Setter Target="RootSplitView.OpenPaneLength" 
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength" 
                        Value="96"/>
                <Setter Target="NavMenuList.Margin" 
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin" 
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle" 
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Um den Auslöser zu erstellen, fügen Sie Ihrer App die folgende Klasse hinzu. Dies ist die Klasse, auf die zuvor durch den XAML-Code verwiesen wird:

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }
        
        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

Nachdem Sie den benutzerdefinierten Auslöser hinzugefügt haben, wird Ihre App automatisch die Layoutänderungen ausführen, die Sie im XAML-Code angegeben haben, wenn sie erkennt, dass sie auf einer Xbox One-Konsole ausgeführt wird.

## Farben

Standardmäßig bewirkt die universelle Windows-Plattform keine Änderung der Farben Ihrer App. Es gibt jedoch Verbesserungen, die für den Satz der von Ihrer App verwendeten Farben durchführen können, um die visuelle Erfahrung auf Fernsehgeräten zu verbessern.

### Anwendungsdesign

Sie können ein **Anwendungsdesign** (dunkel oder hell) wählen, je nach Ihrer App, oder Designs deaktivieren. Weitere allgemeine Empfehlungen für Designs finden Sie in [Farbdesigns](../style/color.md#color-themes).

Die UWP ermöglicht Apps darüber hinaus, das Design dynamisch festzulegen, basierend auf den Systemeinstellungen, die von den Geräten bereitgestellt werden, auf denen sie ausgeführt werden. 
Während die UWP stets die vom Benutzer angegebenen Designeinstellungen beachtet, stellt jedes Gerät auch ein entsprechendes Standarddesign bereit. 
Aufgrund des Zwecks der Xbox One, die eher eine *Medien*- als eine *Produktivitäts*umgebung bereitstellen soll, ist das Systemdesign standardmäßig dunkel. 
Wenn das Design Ihrer App auf den Systemeinstellungen basiert, sollten Sie berücksichtigen, dass es auf Xbox One standardmäßig dunkel ist.

### Akzentfarbe

Die UWP bietet eine bequeme Möglichkeit, die **Akzentfarbe** verfügbar zu machen, die der Benutzer aus den Systemeinstellungen ausgewählt hat.

Auf Xbox One können Benutzer eine Benutzerfarbe auswählen – genauso, wie sie auf einem PC eine Akzentfarbe auswählen können. 
Solange Ihre App diese Akzentfarben über Pinsel oder Farbressourcen aufruft, wird die vom jeweiligen Benutzer in den Systemeinstellungen ausgewählte Farbe verwendet. Beachten Sie, dass Akzentfarben auf Xbox One benutzerbasiert und nicht systembasiert angewendet werden.

Beachten Sie auch, dass der Benutzerfarbensatz auf Xbox One nicht identisch mit dem Benutzerfarbensatz auf PCs, Smartphones und anderen Geräten ist. 
Dies liegt zum Teil daran, dass diese Farben vor allem im Hinblick auf eine optimale 10-Fuß-Erfahrung auf Xbox One ausgewählt wurden, basierend auf in diesem Artikel erklärten Methoden und Strategien.

Solange Ihre App eine Pinselressource wie **SystemControlForegroundAccentBrush** oder eine Farbressource (**SystemAccentColor**) verwendet oder stattdessen Akzentfarben direkt über die [UIColorType.Accent*](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx)-API aufruft, werden diese Farben durch Akzentfarben ersetzt, die für Fernsehgeräte geeignet sind. Pinselfarben mit hohem Kontrast werden wie im Fall von PCs und Telefonen aus dem System abgerufen, jedoch mit Farben, die für Fernsehgeräte geeignet sind.

Weitere Informationen zu Akzentfarben im Allgemeinen finden Sie unter [Akzentfarbe](../style/color.md#accent-color).

### Farbabweichungen zwischen Fernsehgeräten

Beachten Sie bei der Entwicklung von Apps für Fernsehgeräte, dass Farben sehr unterschiedlich dargestellt werden, abhängig von dem Fernsehgerät, auf dem sie gerendert werden. Gehen Sie nicht davon aus, dass die Farben genau wie auf Ihrem Bildschirm angezeigt werden. Wenn Ihre App geringfügige Farbunterschiede nutzt, um Teile der Benutzeroberfläche zu unterscheiden, könnten sich die Farben mischen und die Benutzer könnten verwirrt werden. Verwenden Sie Farben, die unterschiedlich genug sind, dass Benutzer sie unabhängig vom verwendeten Fernsehgerät deutlich unterscheiden können.

### Fernsehsichere Farben

Die RGB-Werte einer Farbe stellen die Intensität für Rot, Grün und Blau dar. Fernsehgeräte behandeln extreme Intensitätsgrade nicht sehr gut. Daher sollten Sie diese Farben bei der Entwicklung für die 10-Fuß-Erfahrung vermeiden. Sie können einen merkwürdigen Bändereffekt hervorrufen oder auf bestimmten Fernsehgeräten verwaschen angezeigt werden. Darüber hinaus verursachen Farben mit hoher Intensität möglicherweise ein „Blooming“, d. h., Pixel in der Nähe beginnen, die gleichen Farben aufzurufen. 

Die Ansichten darüber, was als fernsehsichere Farben gelten kann, gehen zwar auseinander. Im Allgemeinen können Farben mit RGB-Werten zwischen 16 und 235 (oder 10-EB hexadezimal) jedoch sicher für Fernsehgeräte verwendet werden.

![Fernsehsicherer Farbbereich](images/designing-for-tv/tv-safe-colors.png)

### Korrigieren nicht fernsehsicherer Farben

Das Korrigieren nicht fernsehsicherer Farben durch die Anpassung ihrer RGB-Werte, sodass sich diese innerhalb des fernsehsicheren Bereichs befinden, wird in der Regel als **Farbklammerung** (Color Clamping) bezeichnet. Diese Methode ist vielleicht für Apps geeignet, die keine umfassenden Farbpaletten verwenden. Wenn Farben auf diese Weise korrigiert werden, kann dies jedoch dazu führen, dass sie miteinander kollidieren. Dies resultiert in einer nicht optimalen 10-Fuß-Erfahrung.

Stattdessen empfehlen wir Ihnen, zur Optimierung Ihrer Farbpalette für Fernsehgeräte eine **Skalierung** anzuwenden, nachdem Sie mittels eines Verfahrens wie der Farbklammerung sichergestellt haben, dass Ihre Farben fernsehsicher sind. 

<!--[v-lcap] This seems to contradict what you just said in the previous sentence-->

Dies bedeutet, dass alle RGB-Werte Ihrer Farben um einen bestimmten Faktor skaliert werden, damit sich diese innerhalb des fernsehsicheren Bereichs befinden. 
Die Skalierung aller Farben Ihrer App verhindert, dass Farben kollidieren, und sorgt für eine bessere 10-Fuß-Erfahrung.

![Vergleich von Klammerung und Skalierung](images/designing-for-tv/clamping-vs-scaling.png)

### Ressourcen

Stellen Sie sicher, dass Sie auch die Ressourcen entsprechend aktualisieren, wenn Sie Farben ändern. Wenn Ihre App in XAML eine Farbe verwendet, die identisch mit einer Ressourcenfarbe sein soll, Sie jedoch lediglich den XAML-Code aktualisieren, werden die Farben Ihrer Ressourcen abweichen.

### Beispiel für UWP-Farbe

[UWP-Farbdesigns](../style/color.md#color-themes) wurden auf der Basis eines **schwarzen** Hintergrunds für das dunkle Design oder eines **weißen** Hintergrunds für das helle Design Ihrer App entwickelt. Da weder Schwarz noch Weiß fernsehsicher sind, müssen diese Farben mittels *Klammerung* korrigiert werden. Nach ihrer Korrektur müssen alle anderen Farben mittels *Skalierung* angepasst werden, um den erforderlichen Kontrast zu erhalten.

<!--[v-lcap to eliot]why is the above paragraph in the past tense?-->

Der folgende Beispielcode stellt ein Farbdesign bereit, das für die Verwendung auf Fernsehgeräten optimiert wurde:

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="ApplicationPageBackgroundThemeBrush" 
                                 Color="#FF101010"/>
                <Color x:Key="SystemAltHighColor">#FF101010</Color>
                <Color x:Key="SystemAltLowColor">#33101010</Color>
                <Color x:Key="SystemAltMediumColor">#99101010</Color>
                <Color x:Key="SystemAltMediumHighColor">#CC101010</Color>
                <Color x:Key="SystemAltMediumLowColor">#66101010</Color>
                <Color x:Key="SystemBaseHighColor">#FFEBEBEB</Color>
                <Color x:Key="SystemBaseLowColor">#33EBEBEB</Color>
                <Color x:Key="SystemBaseMediumColor">#99EBEBEB</Color>
                <Color x:Key="SystemBaseMediumHighColor">#CCEBEBEB</Color>
                <Color x:Key="SystemBaseMediumLowColor">#66EBEBEB</Color>
                <Color x:Key="SystemChromeAltLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeBlackHighColor">#FF101010</Color>
                <Color x:Key="SystemChromeBlackLowColor">#33101010</Color>
                <Color x:Key="SystemChromeBlackMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeBlackMediumColor">#CC101010</Color>
                <Color x:Key="SystemChromeDisabledHighColor">#FF333333</Color>
                <Color x:Key="SystemChromeDisabledLowColor">#FF858585</Color>
                <Color x:Key="SystemChromeHighColor">#FF767676</Color>
                <Color x:Key="SystemChromeLowColor">#FF1F1F1F</Color>
                <Color x:Key="SystemChromeMediumColor">#FF262626</Color>
                <Color x:Key="SystemChromeMediumLowColor">#FF2B2B2B</Color>
                <Color x:Key="SystemChromeWhiteColor">#FFEBEBEB</Color>
                <Color x:Key="SystemListLowColor">#19EBEBEB</Color>
                <Color x:Key="SystemListMediumColor">#33EBEBEB</Color>
            </ResourceDictionary>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ApplicationPageBackgroundThemeBrush" 
                                 Color="#FFEBEBEB" /> 
                <Color x:Key="SystemAltHighColor">#FFEBEBEB</Color>
                <Color x:Key="SystemAltLowColor">#33EBEBEB</Color>
                <Color x:Key="SystemAltMediumColor">#99EBEBEB</Color>
                <Color x:Key="SystemAltMediumHighColor">#CCEBEBEB</Color>
                <Color x:Key="SystemAltMediumLowColor">#66EBEBEB</Color>
                <Color x:Key="SystemBaseHighColor">#FF101010</Color>
                <Color x:Key="SystemBaseLowColor">#33101010</Color>
                <Color x:Key="SystemBaseMediumColor">#99101010</Color>
                <Color x:Key="SystemBaseMediumHighColor">#CC101010</Color>
                <Color x:Key="SystemBaseMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeAltLowColor">#FF1F1F1F</Color>
                <Color x:Key="SystemChromeBlackHighColor">#FF101010</Color>
                <Color x:Key="SystemChromeBlackLowColor">#33101010</Color>
                <Color x:Key="SystemChromeBlackMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeBlackMediumColor">#CC101010</Color>
                <Color x:Key="SystemChromeDisabledHighColor">#FFCCCCCC</Color>
                <Color x:Key="SystemChromeDisabledLowColor">#FF7A7A7A</Color>
                <Color x:Key="SystemChromeHighColor">#FFB2B2B2</Color>
                <Color x:Key="SystemChromeLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeMediumColor">#FFCCCCCC</Color>
                <Color x:Key="SystemChromeMediumLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeWhiteColor">#FFEBEBEB</Color>
                <Color x:Key="SystemListLowColor">#19101010</Color>
                <Color x:Key="SystemListMediumColor">#33101010</Color>
            </ResourceDictionary> 
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

> **Hinweis**&nbsp;&nbsp;Die hellen Designs **SystemChromeMediumLowColor** und **SystemChromeMediumLowColor** haben absichtlich die gleiche Farbe, nicht infolge von Klammerung. 

<!--[v-lcap] Double check that you didn't mean to say something else in the sentence above-->

> **Hinweis**&nbsp;&nbsp;Hexadezimale Farben werden in **ARGB** (Alpha Rot Grün Blau) angegeben.

Es wird nicht empfohlen, fernsehsichere Farben ohne Klammerung auf einem Monitor zu verwenden, der die gesamte Palette anzeigen kann, da die Farben in diesem Fall verwaschen angezeigt werden. Laden Sie stattdessen das Ressourcenverzeichnis (aus dem vorherigen Beispiel), wenn Ihre App auf Xbox ausgeführt wird, jedoch *nicht* auf anderen Plattformen. Fügen Sie in der `OnLaunched`-Methode von `App.xaml.cs` die folgende Prüfung hinzu:

```csharp
if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
{ 
    this.Resources.MergedDictionaries.Add(new ResourceDictionary 
    { 
        Source = new Uri("ms-appx:///TenFootStylesheet.xaml") 
    }); 
}
```

Dadurch wird sichergestellt, dass unabhängig von dem Gerät, auf dem die App ausgeführt wird, die richtigen Farben angezeigt werden. Dem Benutzer wird so eine bessere und ästhetisch ansprechendere Umgebung bereitgestellt.

<!--### Light dismiss overlay

In order to call the user's attention to the UI elements that the user is currently manipulating with the game controller or remote control, UWP automatically adds a "smoke" layer that covers areas outside of the popup UI when the app is running on Xbox One. This requires no extra work, but is something to keep in mind when designing your UI.
-->

## Gamepad und Fernbedienung

Entsprechend Tastatur und Maus für PCs und der Fingereingabe für Smartphones und Tablets sind Gamepads und Fernbedienungen die Haupteingabegeräte für die 10-Fuß-Erfahrung. 
In diesem Abschnitt werden die Hardwaretasten und ihre Funktionen vorgestellt. 
In [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) und [Mausmodus](#mouse-mode) erfahren Sie, wie Sie Ihre App für die Verwendung dieser Eingabegeräte optimieren.

Die Qualität des Gamepad- und Fernbedienungsverhaltens, das Sie direkt erhalten, ist davon abhängig, wie gut die Tastatur in Ihrer App unterstützt wird. Eine gute Möglichkeit, um sicherzustellen, dass Ihre App gut mit Gamepads/Fernbedienungen funktioniert, besteht darin, sicherzustellen, dass sie gut mit PC-Tastaturen funktioniert, und sie anschließend mit Gamepads/Fernbedienungen zu testen, um die Schwachstellen in der Benutzeroberfläche zu finden.

### Hardwaretasten

In diesem Dokument werden Schaltflächen mit den Namen bezeichnet, die im folgenden Diagramm angegeben werden.

![Diagramm für Gamepad- und Fernbedienungstasten](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

Wie Sie im Diagramm erkennen können, werden einige Schaltflächen auf Gamepads unterstützt, die nicht auf Fernbedienungen unterstützt werden, und umgekehrt. Sie können zwar Tasten verwenden, die nur auf einer Art von Eingabegerät unterstützt werden, um die Navigation in der Benutzeroberfläche zu beschleunigen. Denken Sie jedoch daran, dass deren Verwendung für kritische Interaktionen zu Situationen führen kann, in den der Benutzer nicht mit bestimmten Teilen der Benutzeroberfläche interagieren kann.

Die folgende Tabelle enthält eine Liste aller Hardwaretasten, die von UWP-Apps unterstützt werden, sowie Angaben dazu, welche Eingabegeräte diese unterstützen.

| Taste                    | Gamepad   | Fernbedienung    |
|---------------------------|-----------|-------------------|
| A/Auswahl-Taste           | Ja       | Ja               |
| B/Zurück-Taste             | Ja       | Ja               |
| Steuerkreuz (D-Pad)   | Ja       | Ja               |
| Menü-Taste               | Ja       | Ja               |
| Ansicht-Taste               | Ja       | Ja               |
| X- und Y-Tasten           | Ja       | Nein                |
| Linker Stick                | Ja       | Nein                |
| Rechter Stick               | Ja       | Nein                |
| Linke und rechte Schalter   | Ja       | Nein                |
| Linke und rechte Bumper    | Ja       | Nein                |
| OneGuide-Taste           | Nein        | Ja               |
| Lautstärke-Taste             | Nein        | Ja               |
| Kanal-Taste            | Nein        | Ja               |
| Mediensteuerungs-Tasten     | Nein        | Ja               |
| Stumm-Taste               | Nein        | Ja               |

### Integrierte Tastenunterstützung

Die UWP ordnet automatisch das vorhandene Tastatureingabeverhalten den Gamepad- und Fernbedienungseingaben zu. Die folgende Tabelle zeigt diese integrierten Zuordnungen.

| Tastatur              | Gamepad/Fernbedienung                        |
|-----------------------|---------------------------------------|
| Pfeiltasten            | Steuerkreuz (D-Pad; auch linker Stick auf Gamepads)    |
| Leertaste              | A/Auswahl-Taste                       |
| Eingabe                 | A/Auswahl-Taste                       |
| Escape                | B/Zurück-Taste*                        |

\*Wenn weder das [KeyDown](https://msdn.microsoft.com/library/windows/apps/br208941)- noch das [KeyUp](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.uielement.keyup.aspx)-Ereignis für die Schaltfläche „B“ von der App behandelt werden, wird das [SystemNavigationManager.BackRequested](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.core.systemnavigationmanager.backrequested.aspx)-Ereignis ausgelöst, das zu einer Rückwärtsnavigation innerhalb der App führt.

### Unterstützung für Beschleuniger

Beschleunigertasten sind Tasten, die für die schnellere Navigation in einer Benutzeroberfläche verwendet werden können. Diese Tasten sind jedoch möglicherweise für ein bestimmtes Eingabegerät spezifisch. Denken Sie daher daran, dass nicht alle Benutzer diese Funktionen verwenden können. Tatsächlich sind zurzeit Gamepads die einzigen Eingabegeräte, die Beschleunigerfunktionen für UWP-Apps auf Xbox One unterstützen.

Die folgende Tabelle zeigt die in die UWP integrierte Beschleunigerunterstützung sowie die Unterstützung, die von Ihnen selbst implementiert werden kann. Nutzen Sie diese Verhaltensweisen in Ihrer benutzerdefinierten Benutzeroberfläche, um eine konsistente und benutzerfreundliche Benutzeroberfläche bereitzustellen.

| Interaktion   | Tastatur   | Gamepad      | Integrierte für:  | Empfohlen für: |
|---------------|------------|--------------|----------------|------------------|
| Schwenken       | Keine       | Rechter Stick  | Keine           |      [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) |
| Bild auf/Bild ab  | Bild auf/Bild ab | Linke/rechte Schalter | Keine | `ScrollViewer` und Liste/Raster
| Seite links/rechts | Keine | Linke/rechte Bumper | [Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) | `ScrollViewer`
| Vergrößern/Verkleinern        | STRG +/- | Linke/rechte Schalter | `ScrollViewer` | Ansichten, die Vergrößern/Verkleinern unterstützen

<!--[v-lcap] Had to move this table into markdown to get the links to work
<table>
    <tr>
        <th>Interaction</th>
        <th>Keyboard</th>
        <th>Gamepad</th>
        <th>Built-in for:</th>
        <th>Recommended for:</th>
    </tr>
    <tr>
        <td>Panning</td>
        <td>None</td>
        <td>Right stick</td>
        <td>None</td>
        <td>[ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)</td>
    </tr>
    <tr>
        <td>Page up/down</td>
        <td>Page up/down</td>
        <td>Left/right triggers</td>
        <td>None</td>
        <td>`ScrollViewer` and list/grid</td>
    </tr>
    <tr>
        <td>Page left/right</td>
        <td>None</td>
        <td>Left/right bumpers</td>
        <td>[Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx).</td>
        <td>`ScrollViewer`</td>
    </tr>
    <tr>
        <td>Zoom in/out</td>
        <td>Ctrl +/-</td>
        <td>Left/right triggers</td>
        <td>`ScrollViewer`</td>
        <td>Views that support zooming in and out
    </tr>
</table>-->

## Mausmodus

Wie in [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) beschrieben, wird der Fokus auf Xbox One mittels eines XY-Navigationssystems verschoben, sodass Benutzer den Fokus von Steuerelement zu Steuerelement verschieben können, indem sie nach oben, unten, links und rechts navigieren. 
Einige Steuerelemente wie [WebView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.webview.aspx) und 
[MapControl](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx), 
erfordern jedoch eine mausähnliche Interaktion, bei der Benutzer mit dem Zeiger auf eine beliebige Stelle innerhalb der Grenzen des Steuerelements zeigen können. 
Es gibt auch einige Apps, für die es sinnvoll ist, wenn Benutzer den Zeiger über die gesamte Seite bewegen können, sodass sie mit Gamepads/Fernbedienungen eine Erfahrung ähnlich wie am PC mit einer Maus haben.

Für diese Szenarien sollten Sie einen Zeiger (Mausmodus) für die gesamte Seite oder ein Steuerelement auf einer Seite anfordern. 
Ihre App könnte beispielsweise eine Seite mit einem `WebView`-Steuerelement haben, das den Mausmodus nur innerhalb des Steuerelements und überall sonst eine XY-Fokusnavigation verwendet. 
Um einen Zeiger anzufordern, können Sie angeben, ob er verwendet werden soll, **wenn ein Steuerelement oder eine Seite aktiviert ist** oder **wenn eine Seite den Fokus hat**.

> **Hinweis**&nbsp;&nbsp;Das Anfordern eines Zeigers, wenn ein Steuerelement den Fokus erhält, wird nicht unterstützt.

Das folgende Diagramm zeigt die Tastenzuordnungen für Gamepads/Fernbedienungen im Mausmodus.

![Tastenzuordnungen für Gamepads/Fernbedienungen im Mausmodus](images/designing-for-tv/mouse-mode.png)

> **Hinweis**&nbsp;&nbsp;Der Mausmodus wird nur auf Xbox One mit Gamepad/Fernbedienung unterstützt. Bei anderen Gerätefamilien und Eingabetypen wird er stillschweigend ignoriert.

Verwenden Sie die `RequiresPointer`-Eigenschaft für ein Steuerelement oder eine Seite, um den Mausmodus hierfür zu aktivieren. `RequiresPointer` hat drei mögliche Werte: `Never` (Standardwert), `WhenEngaged` und `WhenFocused`.

> **Hinweis**&nbsp;&nbsp;`RequiresPointer` ist eine neue API und noch nicht dokumentiert. 

<!--TODO: Link to doc-->

### Aktivieren des Mausmodus für ein Steuerelement

Wenn der Benutzer ein Steuerelement mit `RequiresPointer="WhenEngaged"` verwendet, wird der Mausmodus für das Steuerelement aktiviert, bis der Benutzer es nicht mehr verwendet. Der folgende Codeausschnitt zeigt ein einfaches `MapControl`-Steuerelement, das den Mausmodus aktiviert, wenn es verwendet wird:

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true" 
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page> 
```

> **Hinweis**&nbsp;&nbsp;Wenn ein Steuerelement bei Verwendung den Mausmodus aktiviert, muss es auch eine Interaktion mit `IsEngagementRequired="true"` erfordern. Andernfalls wird der Mausmodus nie aktiviert.

Wenn sich ein Steuerelement im Mausmodus befindet, befinden sich dessen verschachtelte Steuerelemente ebenfalls im Mausmodus. Der angeforderte Modus der untergeordneten Elemente wird ignoriert; es ist nicht möglich, dass sich ein übergeordnetes Element im Mausmodus befindet, jedoch nicht dessen untergeordnete Elemente.

Darüber hinaus wird der angeforderte Modus eines Steuerelements nur untersucht, wenn es den Fokus erhält. Daher kann der Modus nicht dynamisch geändert werden, während es den Fokus besitzt.

### Aktivieren des Mausmodus auf einer Seite

Wenn eine Seite die Eigenschaft `RequiresPointer="WhenFocused"` besitzt, wird der Mausmodus für die gesamte Seite aktiviert, wenn sie den Fokus erhält. Der folgende Codeausschnitt zeigt, wie Sie einer Seite diese Eigenschaft zuweisen:

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page> 
```

> **Hinweis**&nbsp;&nbsp;Der `WhenFocused`-Wert wird nur für [Page](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page.aspx)-Objekte unterstützt. Wenn Sie versuchen, diesen Wert für ein Steuerelement festzulegen, wird eine Ausnahme ausgelöst.

## Fokusanzeige

Die Fokusanzeige ist der Rahmen um das Benutzeroberflächenelement, das zurzeit den Fokus besitzt. Dies hilft Benutzern, sich zu orientieren, damit sie in Ihrer Benutzeroberfläche einfach navigieren können, ohne die Orientierung zu verlieren.

Dank der Hinzufügung einer Anzeigenaktualisierung und zahlreicher Anpassungsoptionen zur Fokusanzeige können Entwickler darauf vertrauen, dass eine einzelne Fokusanzeige gut auf PCs und Xbox One und allen anderen Windows 10-Geräten funktioniert, die Tastaturen und/oder Gamepads/Fernbedienungen unterstützen.

Während die gleiche Fokusanzeige auf verschiedenen Plattformen verwendet werden kann, unterscheidet sich der Kontext, in dem der Benutzer diese antrifft, für die 10-Fuß-Erfahrung etwas. Sie sollten davon ausgehen, dass der Benutzer nicht dem gesamten Fernsehbildschirm seine volle Aufmerksamkeit schenkt und es daher wichtig ist, dass das aktuell fokussierte Element für den Benutzer jederzeit deutlich erkennbar ist, um eine frustrierende Suche nach der Anzeige zu vermeiden.

Darüber hinaus ist es wichtig, zu beachten, dass die Fokusanzeige standardmäßig angezeigt wird, wenn ein Gamepad oder eine Fernbedienung verwendet wird, jedoch *nicht*, wenn eine Tastatur verwendet wird. Auch wenn Sie keine Fokusanzeige implementieren, wird sie daher angezeigt, wenn Sie Ihre App auf Xbox One ausführen.

### Anfängliche Platzierung der Fokusanzeige

Platzieren Sie beim Starten einer App oder Navigieren zu einer Seite den Fokus auf einem Benutzeroberflächenelement, das sinnvollerweise als erstes Element betrachtet werden kann, für das Benutzer eine Aktion ausführen würden. Beispielsweise könnte eine Foto-App den Fokus auf das erste Element im Katalog platzieren, und eine Musik-App, in der zur detaillierten Ansicht eines Musiktitels navigiert wurde, könnte den Fokus auf die Abspieltaste setzen, um eine einfache Wiedergabe der Musik zu ermöglichen.

Platzieren Sie den anfänglichen Fokus in Ihrer App möglichst in den Bereich oben links (oder oben rechts bei einem Lesefluss von rechts nach links). Die meisten Benutzer neigen dazu, sich zunächst auf diesen Bereich zu konzentrieren, da der Inhaltsfluss einer App im Allgemeinen dort beginnt.

### Klare Erkennbarkeit des Fokus

Eine Fokusanzeige sollte stets auf dem Bildschirm sichtbar sein, damit Benutzer Vorgänge an der Stelle fortsetzen können, an der sie aufgehört haben, ohne nach dem Fokus suchen zu müssen. Entsprechend sollte sich stets ein fokussierbares Element auf dem Bildschirm befinden. Verwenden Sie beispielsweise keine Popups, die nur Text und keine fokussierbaren Elemente enthalten.

### Overlay für einfaches Ausblenden

Um die Aufmerksamkeit der Benutzer auf die Benutzeroberflächenelemente zu lenken, die sie gerade mit dem Gamecontroller oder der Fernbedienung bearbeiten, fügt die UWP automatisch eine „Rauchschicht“ hinzu, die Bereiche außerhalb der Popup-Benutzeroberfläche bedeckt, wenn die App auf Xbox One ausgeführt wird. Dies erfordert keinen zusätzlichen Aufwand. Sie sollten dies während der Entwicklung Ihrer Benutzeroberfläche jedoch berücksichtigen.

## Fokusaktivierung

Die Fokusaktivierung soll die Verwendung eines Gamepads oder einer Fernbedienung für Interaktionen mit einer App vereinfachen. 

> **Hinweis**&nbsp;&nbsp;Das Einrichten der Fokusaktivierung wirkt sich nicht auf Tastaturen oder andere Eingabegeräte aus.

Wenn die Eigenschaft `IsFocusEngagementEnabled` für ein [FrameworkElement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) -Objekt auf `True` festgelegt wird, zeigt dies an, dass das Steuerelement Fokusaktivierung erfordert. Das bedeutet, dass der Benutzer die **A/Select**-Taste (Auswahl-Taste) drücken muss, um das Steuerelement zu „aktivieren“ und mit diesem zu interagieren. Anschließend können sie die **B/Back**-Taste (Zurück-Taste) drücken, um das Steuerelement zu deaktivieren und von diesem weg zu navigieren.

> **Hinweis**&nbsp;&nbsp;`IsFocusEngagementEnabled` ist eine neue API und noch nicht dokumentiert.

### Fokustrapping

Fokustrapping bedeutet, dass ein Benutzer versucht, in der Benutzeroberfläche einer App zu navigieren, jedoch innerhalb eines Steuerelements „gefangen“ ist, wodurch es schwer oder sogar unmöglich wird, von diesem Steuerelement weg zu navigieren.

Das folgende Beispiel zeigt eine Benutzeroberfläche, die ein Fokustrapping verursacht.

![Schaltflächen links und rechts neben einem horizontalen Schieberegler](images/designing-for-tv/focus-engagement-focus-trapping.png)

Wenn der Benutzer von der linken zur rechten Schaltfläche navigieren möchte, wäre es logisch, anzunehmen, dass er hierzu auf dem Steuerkreuz (D-Pad) oder linken Stick lediglich zweimal rechts drücken muss. 
Wenn das [Slider](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.slider.aspx)-Steuerelement jedoch keine Aktivierung erfordert, würde das folgende Verhalten eintreten; Wenn der Benutzer das erste Mal rechts drückt, würde der Fokus zum `Slider` verschoben. Wenn er das zweite Mal rechts drückt, würde der Ziehpunkt des `Slider` nach rechts verschoben. Der Benutzer würde den Ziehpunkt immer weiter nach rechts verschieben und könnte nicht zur Schaltfläche gelangen.

Es gibt verschiedene Methoden, um dieses Problem zu umgehen. Eine Möglichkeit besteht darin, ein anderes Layout ähnlich wie im Beispiel für eine Immobilien-App in [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) zu entwerfen. Dort wurden die Schaltflächen **Zurück** und **Weiter** oberhalb der `ListView` platziert. Eine vertikale anstelle einer horizontalen Anordnung der Steuerelemente wie in der folgenden Abbildung würde das Problem lösen.

![Schaltflächen ober- und unterhalb eines horizontalen Schiebereglers](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

Nun kann der Benutzer zu jedem der Steuerelemente navigieren, indem er auf dem Steuerkreuz (D-Pad/linken Stick) nach oben und nach unten drückt. Wenn der `Slider` den Fokus hat, kann er links und rechts drücken, um den Ziehpunkt des `Slider` zu verschieben, wie erwartet.

Ein anderer Ansatz zur Lösung dieses Problems besteht darin, für den `Slider` Aktivierung zu erfordern. Wenn Sie `IsFocusEngagementEnabled="True"` festlegen, führt dies zum folgenden Verhalten.

![Anfordern einer Fokusaktivierung für den Schieberegler, damit Benutzer zur Schaltfläche auf der rechten Seite navigieren können](images/designing-for-tv/focus-engagement-slider.png)

Wenn der `Slider` Fokusaktivierung erfordert, kann der Benutzer einfach zur Schaltfläche auf der rechten Seite gelangen, indem er auf dem Steuerkreuz (D-Pad)/dem linken Stick zweimal rechts drückt. Diese Lösung ist hervorragend geeignet, da sie keine Benutzeroberflächenanpassung erfordert und das erwartete Verhalten erzeugt.

### Elementsteuerelemente

Abgesehen vom [Slider](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.slider.aspx)-Steuerelement gibt es weitere Steuerelemente, für die Sie möglicherweise eine Aktivierung anfordern sollten, wie beispielsweise:

- [ListBox](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)
- [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [GridView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)
- [FlipView](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.flipview)

Im Gegensatz zum `Slider`-Steuerelement, fangen diese Steuerelemente den Fokus nicht innerhalb ihrer selbst. Sie können jedoch Probleme hinsichtlich der Verwendbarkeit verursachen, wenn sie große Mengen an Daten enthalten. Im Folgenden finden Sie ein Beispiel für eine `ListView`, die eine große Menge an Daten enthält.

![ListView mit einer großen Menge an Daten und Schaltflächen ober- und unterhalb](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

Versuchen wir, ähnlich wie im Beispiel für das `Slider`-Steuerelement mit einem Gamepad/einer Fernbedienung von der Schaltfläche oben zur Schaltfläche unten zu navigieren. 
Zunächst hat die Schaltfläche oben den Fokus. Wenn wir auf dem Steuerkreuz (D-Pad)/dem Stick nach unten drücken, wird der Fokus auf das erste Element in der `ListView` („Element 1“) platziert. 
Wenn der Benutzer erneut nach unten drückt, erhält das nächste Element in der Liste den Fokus, nicht die Schaltfläche unten. 
Um zur Schaltfläche zu gelangen, muss der Benutzer zunächst durch jedes Element in der `ListView` navigieren. 
Wenn die `ListView` eine große Menge an Daten enthält, kann dies umständlich sein und stellt keine optimale Benutzererfahrung dar.

Um dieses Problem zu lösen, legen Sie die Eigenschaft `IsFocusEngagementEnabled="True"` für `ListView` fest, um Aktivierung zu erfordern. 
Dadurch können Benutzer die `ListView` schnell überspringen, indem sie einfach nach unten drücken. Sie können jedoch 
keinen Bildlauf durch die Liste ausführen oder ein Element aus der Liste auswählen, wenn sie diese nicht durch Drücken der **A/Select**-Taste (Auswahl-Taste) aktivieren, wenn die Liste den Fokus hat. Anschließend können sie die **B/Back**-Taste (Zurück-Taste) drücken, um die Liste zu deaktivieren.

![ListView mit erforderlicher Aktivierung](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### ScrollViewer

[ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) unterscheidet sich etwas von diesen Steuerelementen. 
Es weist spezifische Besonderheiten auf, die Sie berücksichtigen müssen. Wenn Sie ein `ScrollViewer`-Steuerelement mit fokussierbarem Inhalt besitzen, können Sie standardmäßig durch Navigieren zum `ScrollViewer`-Steuerelement durch dessen fokussierbare Elemente navigieren. Wie in einer `ListView`, müssen Sie einen Bildlauf über alle Elemente ausführen, um von `ScrollViewer` weg zu navigieren. 

Wenn `ScrollViewer` *keine* fokussierbaren Inhalte besitzt, also beispielsweise lediglich Text enthält, können Sie `IsFocusEngagementEnabled="True"` festlegen, sodass Benutzer `ScrollViewer` mithilfe der **A/Select**-Taste (Auswahl-Taste) aktivieren können. Nach der Aktivierung können sie mit dem **Steuerkreuz (D-Pad)/linken Stick** einen Bildlauf durch den Text ausführen und anschließend die **B/Back**-Taste (Zurück-Taste) drücken, um das Steuerelement zu deaktivieren, wenn sie den Vorgang abgeschlossen haben.

Ein weiterer Ansatz besteht darin, `IsTabStop="True"` für `ScrollViewer` festzulegen, damit der Benutzer das Steuerelement nicht aktivieren muss. Er kann in diesem Fall 
einfach den Fokus auf das Steuerelement setzen und anschließend mit dem **Steuerkreuz (D-Pad)/linken Stick** einen Bildlauf ausführen, wenn es innerhalb von `ScrollViewer` keine fokussierbaren Elemente gibt.

### Standardeinstellungen in Bezug auf die Fokusaktivierung

Einige Steuerelemente führen häufig genug dazu, dass Benutzer in einem Steuerelement gefangen werden, um die Fokusaktivierung als Standardeinstellung zu rechtfertigen. Im Fall anderer Steuerelemente, für die die Fokusaktivierung standardmäßig deaktiviert ist, kann es nützlich sein, die Fokusaktivierung zu aktivieren. Die folgende Tabelle listet diese Steuerelemente und deren Standardverhalten in Bezug auf die Fokusaktivierung auf.

| Steuerelement               | Standardeinstellung in Bezug auf die Fokusaktivierung  |
|-----------------------|---------------------------|
| CalendarDatePicker    | Ein                        |
| FlipView              | Aus                       |
| GridView              | Aus                       |
| ListBox               | Aus                       |
| ListView              | Aus                       |
| ScrollViewer          | Aus                       |
| SemanticZoom          | Aus                       |
| Slider                | Ein                        |

Alle anderen UWP-Steuerelemente zeigen keine Änderungen in Bezug auf Verhalten oder Anzeige, wenn `IsFocusEngagementEnabled="True"` festgelegt wird.

## XY-Fokusnavigation und -interaktion

Wenn Ihre App eine ordnungsgemäße Fokusnavigation für Tastaturen unterstützt, wird dies gut auf Gamepads und Fernbedienungen übertragen. 
Die Pfeiltastennavigation ist dem **Steuerkreuz (D-Pad)** (und dem **linken Stick** auf Gamepads) zugeordnet. Interaktionen mit Benutzeroberflächenelementen sind dem Schlüssel **Eingabe/Auswahl** zugeordnet. 
(siehe [Gamepad und Fernbedienung](#gamepad-and-remote-control)). Anleitungen zum Tastaturdesign finden Sie unter [Tastaturinteraktionen](keyboard-interactions.md).

Wenn die Tastaturunterstützung ordnungsgemäß implementiert ist, wird Ihre App angemessen funktionieren. Es sind jedoch möglicherweise zusätzliche Arbeiten erforderlich, um jedes Szenario zu unterstützen. Denken Sie über die spezifischen Anforderungen Ihrer App nach, um die bestmögliche Benutzererfahrung bereitzustellen.

<!--### Focus placement

The focus visual should be initially placed on the UI element that makes sense as the first element on which the user would take action. For more information, see [Focus visual](#focus-visual).
-->

### Nicht zugängliche Benutzeroberfläche

Da die XY-Fokusnavigation den Benutzer auf die Navigation nach oben, unten, links und rechts einschränkt, gibt es möglicherweise Szenarien, in denen auf Teile der Benutzeroberfläche nicht zugegriffen werden kann. 
Das folgende Diagramm zeigt ein Beispiel für die Art von Benutzeroberflächenlayout, das die XY-Fokusnavigation nicht unterstützt. 
Beachten Sie, dass das Element in der Mitte bei Verwendung von Gamepads/Fernbedienungen nicht zugänglich ist, da die vertikale und horizontale Navigation Priorität erhält und die Priorität des mittleren Elements nie hoch genug sein wird, um den Fokus erhalten.

![Elemente in vier Ecken mit nicht zugänglichem Element in der Mitte](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

Wenn die Benutzeroberfläche aus irgendeinem Grund nicht neu angeordnet werden kann, verwenden Sie eine der im nächsten Abschnitt erläuterten Techniken, um das Standardfokusverhalten zu überschreiben.

### Überschreiben der Standardnavigation <a name="overriding-the-default-navigation"></a>

Während die UWP versucht, sicherzustellen, dass die Navigation mit dem Steuerkreuz (D-Pad)/linken Stick für den Benutzer sinnvoll ist, kann sie keine Verhaltensweisen garantieren, die für die Ziele Ihrer App optimiert sind. 
Die beste Möglichkeit, um sicherzustellen, dass die Navigation für Ihre App optimiert ist, besteht im Testen mit einem Gamepad. So können Sie überprüfen, ob Benutzer auf alle Benutzeroberflächenelemente auf eine Weise zugreifen können, die für die Szenarien Ihrer App sinnvoll ist. Wenn die Szenarien Ihrer App Verhaltensweisen erfordern, die durch die vorhandene XY-Fokusnavigation nicht bereitgestellt werden können, ziehen Sie die Empfehlungen in den folgenden Abschnitten in Betracht und/oder überschreiben das Verhalten, um den Fokus auf ein logisches Element zu setzen.

Der folgende Codeausschnitt zeigt, wie Sie das Verhalten der XY-Fokusnavigation überschreiben können:

```xml
<StackPanel>
    <Button x:Name="MyBtnLeft" 
            Content="Search" />
    <Button x:Name="MyBtnRight" 
            Content="Delete"/>
    <Button x:Name="MyBtnTop" 
            Content="Update" />
    <Button x:Name="MyBtnDown" 
            Content="Undo" />
    <Button Content="Home"  
            XYFocusLeft="{x:Bind MyBtnLeft}" 
            XYFocusRight="{x:Bind MyBtnRight}"
            XYFocusDown="{x:Bind MyBtnDown}"
            XYFocusUp="{x:Bind MyBtnTop}" />
</StackPanel>
```

Wenn sich der Fokus auf der `Home`-Schaltfläche und der Benutzer nach links navigiert, wird der Fokus zur `MyBtnLeft`-Schaltfläche verschoben. Wenn der Benutzer nach rechts navigiert, wird der Fokus zur `MyBtnRight`-Schaltfläche verschoben usw.

Um zu verhindern, dass der Fokus von einem Steuerelement in eine bestimmten Richtung verschoben wird, verwenden Sie die `XYFocus*`-Eigenschaft, um auf das gleiche Steuerelement zu zeigen:

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

### Pfad der wenigsten Klicks <a name="path-of-least-clicks"></a>

Ermöglichen Sie Benutzern, die am häufigsten ausgeführten Aktionen mit der geringsten Anzahl von Klicks auszuführen. Im folgenden Beispiel befindet sich [TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) zwischen der **Play**-Schaltfläche (die zunächst den Fokus erhält) und einem häufig verwendeten Element, sodass sich zwischen zwei Aktionen mit Priorität ein nicht notwendiges Element befindet.

![Bewährte Methoden für die Navigation stellen den Pfad der wenigsten Klicks bereit](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

Im folgenden Beispiel befindet sich [TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) stattdessen oberhalb der **Play**-Schaltfläche. 
Durch das einfache Neuanordnen der Benutzeroberfläche, sodass sich nicht notwendige Elemente nicht zwischen Aktionen mit Priorität befinden, wird die Verwendbarkeit Ihrer App erheblich verbessert.

![TextBlock an eine Stelle oberhalb der Play-Schaltfläche verschoben, sodass sie sich nicht mehr zwischen Aktionen mit Priorität befindet](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

<!--### Nested UI elements

When UI elements are nested inside other UI elements, the default behavior is that the user will not be able to access the nested UI elements.

One of the main scenarios is when there is UI that displays when the user hovers over a nested UI element with the mouse, but does not display otherwise.

![UI elements displaying when mouse hovers over them](images/designing-for-tv/2d-navigation-best-practices-ui-elements-display-on-mouse-hover.png)

The recommended way to handle this scenario for gamepad/remote input is to place these UI elements in a `ContextFlyout` that displays when the user presses the **Menu** button.
-->
<!--#### UI elements that always display

The second scenario is when there is UI that always displays. **TODO: Fill in this section when I get more info**
-->

### CommandBar und ContextFlyout

Bei Verwendung einer [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) müssen Sie das Problem hinsichtlich Bildläufen durch Listen berücksichtigen, wie in [Problem: Benutzeroberflächenelemente nach langen bildlauffähigen Listen/Rastern](#problem-ui-elements-located-after-long-scrolling-list-grid) beschrieben. Die folgende Abbildung zeigt ein Benutzeroberflächenlayout mit `CommandBar` am Ende einer Liste/eines Rasters. Der Benutzer müsste einen Bildlauf ganz nach unten durch die Liste/das Rasters ausführen, um zu `CommandBar` zu gelangen.

![CommandBar am unteren Ende einer Liste/eines Rasters](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

Was geschieht, wenn Sie `CommandBar` an einer Stelle *oberhalb* der Liste/des Rasters platzieren? Auch wenn ein Benutzer, der einen Bildlauf nach unten durch die Liste/das Raster ausführt, einen Bildlauf zurück nach oben ausführen müsste, um zu `CommandBar` zu gelangen, bedeutet dies etwas weniger Navigationsaufwand als bei der vorherigen Konfiguration. Beachten Sie, dass dies voraussetzt, dass sich der anfängliche Fokus Ihrer App neben oder oberhalb von `CommandBar` befindet. Dieser Ansatz funktioniert weniger gut, wenn sich der anfängliche Fokus unterhalb der Liste/des Rasters befindet. Wenn diese `CommandBar`-Elemente globale Aktionselemente sind, auf die nicht sehr häufig zugegriffen werden muss (wie beispielsweise eine **Sync**-Schaltfläche), ist es möglicherweise zulässig, diese oberhalb der Liste/des Rasters zu platzieren.

Wenn Ihre App ein `CommandBar`-Steuerelement besitzt, auf dessen Elemente Benutzer leicht zugreifen können müssen, sollten Sie es in Betracht ziehen, diese Elemente innerhalb eines `ContextFlyout`-Elements zu platzieren und aus der `CommandBar` zu entfernen. 

<!--The `ContextFlyout` can be accessed by pressing the **Menu** button, providing a very convenient way for the user to access these actions quickly.-->

Zwar ist das vertikale Stapeln der `CommandBar`-Elemente nicht möglich, die Platzierung gegen die Bildlaufrichtung (etwa links oder rechts von einer vertikal laufenden Liste oder über/unter einer horizontal laufenden Liste) ist eine weitere Option, die Sie nutzen können, wenn dies gut zu Ihrem UI-Layout passt.

<!--### Navigation pane
A navigation pane (also known as a *hamburger menu*) is a navigation control commonly used in UWP apps. Typically it is a pane with several options to choose from in a list-style menu that will take the user to different pages. Generally this pane starts out collapsed to save space, and the user can open it by clicking on a button.
While nav panes are very accessible with mouse and touch, gamepad/remote makes them less accessible since the user has to navigate to a button to open the pane. Therefore, a good practice is to have the **Menu** button open the nav pane, as well as allow the user to open it by navigating all the way to the left of the page. This will provide the user with very easy access to the contents of the pane. See [Nav panes](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane). TO DO: Is this the right link?--> 
<!--TODO: Image of nav pane-->

### Herausforderungen beim UI-Layout

Einige UI-Layouts sind aufgrund der Natur der XY-Fokus-Navigation anspruchsvoller und sollten jeweils einzeln für sich beurteilt werden. Es gibt zwar keine einzige „richtige“ Lösung, und Ihre Entscheidung muss auf den Anforderungen Ihrer App basieren, es stehen jedoch einige Techniken zur Verfügung, die Ihnen dabei helfen können, ein hervorragendes TV-Erlebnis bereitzustellen.

Um dies besser zu verstehen, sehen wir uns eine imaginäre App an, die einige dieser Herausforderungen sowie die Techniken, um sie zu bewältigen, illustriert.

> **Hinweis**&nbsp;&nbsp;Diese fiktive App dient dazu, UI-Probleme und mögliche Lösungen dafür zu illustrieren, und sie zeigt nicht den optimalen Benutzerkomfort für Ihre App.

Nachfolgend sehen Sie eine fiktive Immobilien-App, die eine Liste zum Verkauf stehender Häuser, eine Karte, Beschreibungen von Immobilien sowie weitere Informationen anzeigt. Diese App stellt Sie vor drei Herausforderungen, denen Sie mit den folgenden Techniken begegnen können:

- [Neuanordnung der UI](#ui-rearrange)
- [Fokusaktivierung](#engagement)
- [Mausmodus](#mouse-mode)

![Fiktive Immobilien-App](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### Problem: UI-Elemente, die sich hinter einer langen Bildlaufliste oder einem Raster befinden <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

Die [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx) der Eigenschaften in der folgenden Abbildung ist eine sehr lange Bildlaufliste. Wenn [Engagement](#focus-engagement) *nicht* für die `ListView` erforderlich ist, wenn der Benutzer zu der Liste navigiert, wird der Fokus auf das erste Element in der Liste gesetzt. Damit der Benutzer zur Schaltfläche **Zurück** oder **Weiter** gelangen kann, muss er alle Elemente der Liste durchlaufen. In Fällen wie diese, bei denen der Benutzer die gesamte Liste mühsam durchlaufen muss – d.h. wenn die Liste ganz einfach zu lang ist, um noch akzeptablen Benutzerkomfort zu ermöglichen – sollten Sie andere Optionen erwägen.

![Immobilien-App: Eine Liste mit 50 Elementen erfordert 51 Klicks, bis die Schaltflächen am Ende erreicht sind](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### Lösungen

##### Neuanordnung der UI <a name="ui-rearrange"></a>

Sofern nicht der anfängliche Fokus auf das Ende der Seite gesetzt ist, sind über einer langen Bildlaufliste platzierte UI-Elemente typischerweise einfacher erreichbar, als solche unterhalb einer solchen Liste. 
Wenn dieses neue Layout für andere Geräte funktioniert, kann das Ändern des Layouts für alle Gerätefamilien kostengünstiger sein als das Vornehmen spezieller UI-Änderungen nur für die Xbox One. 
Weiterhin gilt, dass die Platzierung von UI-Elementen gegen die Bildlaufrichtung (d.h. horizontal bei einer vertikal laufenden Liste oder vertikal bei einer horizontal laufenden Liste) den Benutzerkomfort noch weiter erhöht.

![Immobilien-App: Platzieren von Schaltflächen oberhalb einer langen Bildlaufliste](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

##### Fokusaktivierung <a name="engagement"></a>

Ist die Aktivierung *erforderlich*, wird die gesamte `ListView` zu einem einzigen Fokusziel. Der Benutzer kann die Inhalte der Liste übergehen, um zum nächsten fokussierbaren Element zu gelangen. Erfahren Sie mehr darüber, welche Steuerelemente die Aktivierung unterstützen, und wie Sie sie in der [Fokusaktivierung](#focus-engagement) verwenden können.

![Immobilien-App: Einstellen der Aktivierung als erforderlich, damit die Schaltflächen „Zurück/Weiter“ mit nur einem Klick erreicht werden können](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### Problem: ScrollViewer ohne fokussierbare Elemente

Da die XY-Fokus-Navigation darauf basiert, dass jeweils zu einem fokussierbaren UI-Element navigiert wird, 
kann ein [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) ohne fokussierbare Elemente (etwa einer, der, wie der in diesem Beispiel, nur Text enthält) ein Szenario verursachen, in dem der Benutzer nicht alle Inhalte in dem `ScrollViewer` anzeigen kann. 
Lösungen für dieses und ähnliche Szenarien finden Sie unter [Fokusaktivierung](#focus-engagement).

![Immobilien-App: ScrollViewer mit lediglich Text](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### Problem: UI mit freiem Bildlauf

Wenn Ihre App eine UI mit freiem Bildlauf benötigt, etwa eine Zeichenoberfläche oder, wie in diesem Beispiel, eine Karte, ist die XY-Fokus-Navigation ganz einfach nicht geeignet. 
In solchen Fällen können Sie den [Mausmodus](#mouse-mode) aktivieren, damit der Benutzer in einem UI-Element frei navigieren kann.

![Karten-UI-Element im Mausmodus](images/designing-for-tv/map-mouse-mode.png)

<!--## 2D navigation best practices

Since 2D navigation only lets the user navigate up, down, left, and right, but not diagonally, certain UI layouts work better than others. This section describes some common issues related to navigation and their recommended solutions. Please note that there is no single "right" way to solve these problems, and always think about how to solve them for your app's specific scenarios.

### UI layouts to avoid

The following diagram illustrates an example of the kind of UI layout that 2D navigation doesn't support. Note that the element in the middle is not accessible using gamepad/remote because the vertical and horizontal navigation will be prioritized and the middle element will never be high enough priority to get focus.

![Elements in four corners with inaccessible element in middle](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

If for some reason rearranging the UI is not possible, use one of the techniques discussed in [Overriding the default navigation](#overriding-the-default-navigation) to override the default focus behavior.

### CommandBar and ContextFlyout

When using a [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), keep in mind the issue of scrolling through a list as mentioned in [Problem: UI elements located after long scrolling list/grid](#problem-ui-elements-located-after-long-scrolling-list-grid). The following image shows a UI layout with the `CommandBar` on the bottom of a list/grid. The user would need to scroll all the way down through the list/grid to reach the `CommandBar`.

![CommandBar at bottom of list/grid](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

Now compare this to the following configuration, which puts the `CommandBar` *above* the list/grid. While a user who scrolled down the list/grid would have to scroll back up to reach the `CommandBar`, it is slightly less navigation than the previous configuration. Note that this is assuming that your app's initial focus is placed next to or above the `CommandBar`; this approach won't work as well if the initial focus is below the list/grid. If these `CommandBar` items are global action items that don't have to be accessed very often (such as a **Sync** button), it may be acceptable to have them above the list/grid as in this example.

![CommandBar above list/grid](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout-2.png)

If your app has a `CommandBar` whose items need to be readily accessible by users, you may want to consider placing these items inside a `ContextFlyout` and removing them from the `CommandBar`. The `ContextFlyout` can be accessed by pressing the **Menu** button, providing a very convenient way for the user to access these actions quickly.

While you can't stack a `CommandBar`'s items vertically, placing them against the scroll direction (for example, to the left or right of a vertically scrolling list, or the top or bottom of a horizontally scrolling list) is another option you may want to consider if it works well for your UI layout.

### Path of least clicks <a name="path-of-least-clicks"></a>

Try to allow the user to perform the most common tasks in the least number of clicks. In the following example, there is a [TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) between the **Play** button and a commonly used element, adding an extra click to get from the initially focused element to the commonly used element.

![TextBlock adds extra click to get to commonly used element](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

Simply rearranging the UI so that unnecessary elements are not placed in between priority tasks will greatly improve your app's usability.

![TextBlock moved above Play button so that it is no longer between priority tasks](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### Nested UI elements

When UI elements are nested inside other UI elements, the default behavior is that the user will not be able to access the nested UI elements. There are two main scenarios involving nested UI, and the solutions for them are slightly different.

#### UI elements that display on mouse hover

The first scenario is when there is UI that displays when the user hovers over an element with the mouse, but does not display otherwise.

![UI elements displaying when mouse hovers over them](images/designing-for-tv/2d-navigation-best-practices-ui-elements-display-on-mouse-hover.png)

The recommended way to handle this scenario for gamepad/remote input is to place these UI elements in a `ContextFlyout` that displays when the user presses the **Menu** button.

#### UI elements that always display

The second scenario is when there is UI that always displays. **TODO: Fill in this section when I get more info**

### Navigation pane

A navigation pane (also known as a *hamburger menu*) is a navigation control commonly used in UWP apps. Typically it is a pane with several options to choose from in a list-style menu that will take the user to different pages. Generally this pane starts out collapsed to save space, and the user can open it by clicking on a button.

While nav panes are very accessible with mouse and touch, gamepad/remote makes them less accessible since the user has to navigate to a button to open the pane. Therefore, a good practice is to have the **Menu** button open the nav pane, as well as allow the user to open it by navigating all the way to the left of the page. This will provide the user with very easy access to the contents of the pane. See [Nav panes](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane) TODO: Is this the right link? for more information.

TODO: Image of nav pane
-->

## Richtlinien für Benutzeroberflächensteuerelemente

Die universelle Windows-Plattform bietet viele Funktionen, die den Benutzerkomfort auf allen Geräten verbessern, und einige davon sind besonders gut für die 10-Fuß-Erfahrung geeignet. Die nachfolgende Liste beschreibt Beispiele für solche Verbesserungen, die Sie an der UI Ihrer App vornehmen können.

### Pivotsteuerelement

Das Steuerelement [Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) hat Eigenschaften, die Sie einstellen können, um zu verhindern, dass Kopfzeilen den Bildschirm umschließen, wie dies in Telefon- und Tabletbildschirmen der Fall ist. Dies ist besser für große Geräte mit großen Bildschirmanzeigen wie beispielsweise Fernsehgeräte geeignet, da dieser Vorgang Benutzer stark ablenken kann. Weitere Informationen finden Sie unter [Registerkarten und Pivots](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tabs-pivot).

### Navigationsbereich

Die UWP ermöglicht ein konsistentes Erscheinungsbild auf allen Geräten. Weitere Informationen zum Verhalten von Navigationsbereichen auf unterschiedlichen Bildschirmgrößen sowie zu bewährten Vorgehensweisen für die Gamepad/Remote-Navigation finden Sie unter [Navigationsbereiche](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/nav-pane).

### CommandBar-Beschriftungen

Das Steuerelement [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) verfügt über eine Eigenschaft, die erzwingt, dass Beschriftungen neben Symbolen immer angezeigt werden. Dies funktioniert gut für die 10-Fuß-Umgebung, da dadurch die Anzahl der Klicks minimiert wird, die der Benutzer ausführen muss, um die Funktionen die Schaltflächen anzuzeigen. Auch für andere Gerätetypen ist dies ein hervorragendes Konzept.

### QuickInfo

Das Steuerelement [QuickInfo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.tooltip.aspx) wurde eingeführt, um zusätzliche Informationen in der Benutzeroberfläche anzeigen zu können, wenn der Benutzer mit der Maus auf ein Element zeigt oder mit dem Finger auf ein Element tippt und den Finger darauf hält. Für Gamepad und Remote wird `Tooltip` kurz nachdem das Element den Fokus erhält angezeigt, bleibt für einen kurzen Zeitraum auf dem Bildschirm und verschwindet dann. Dieses Verhalten könnte zu stark ablenkend wirken, wenn zu viele `Tooltip`s verwendet werden. Versuchen Sie, bei Entwürfen für Fernsehgeräte QuickInfo eher zu vermeiden.

### Schaltflächenstile

Zwar funktionieren die Standard-UWP-Schaltflächen sehr gut auf TV-Bildschirmen, es gibt jedoch einige visuelle Stile für Schaltflächen, die noch besser auf die Benutzeroberfläche aufmerksam machen. Sie sollten diese für alle Plattformen in Erwägung ziehen, besonders für die 10-Fuß-Umgebung, bei der es sehr nützlich ist, den Ort des Fokus klar und deutlich zu machen. Weitere Informationen zu diesen Stilen finden Sie unter [Schaltflächen](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/buttons).

<!--### Sort and filter lists and grids

The visual style for sorting and filtering list and grid items on UWP has been updated for 10-foot scenarios. Read more about how to use this UI functionality [here](TODO: Add link).
-->

### Geschachtelte UI-Elemente

Wenn UI-Elemente in anderen UI-Elementen geschachtelt sind, besteht das Standardverhalten darin, dass der Benutzer nicht auf die geschachtelten UI-Elemente zugreifen kann.

Eines der häufigsten Szenarien involviert eine UI, die angezeigt wird, wenn der Benutzer mit der Maus auf ein geschachteltes UI-Element zeigt, und die auf andere Weise nicht sichtbar gemacht werden kann.

![UI-Elemente, die angezeigt werden, wenn mit der Maus darauf gezeigt wird](images/designing-for-tv/2d-navigation-best-practices-ui-elements-display-on-mouse-hover.png)

Die empfohlene Vorgehensweise für den Umgang mit diesem Szenario für die Gamepad/Remote-Eingabe besteht darin, diese UI-Elemente in ein `ContextFlyout` zu setzen.

<!--[v-lcap] This doc just ends abruptly. I recommended adding a short summary as well as links to related topics, or at least to the parent topic in this set or the next level up-->

<!--### Drag and drop

TODO: Are we including this?
-->


<!--HONumber=Apr16_HO3-->


