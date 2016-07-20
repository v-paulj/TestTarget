---
author: Xansky
Description: "Beschreibt die Schritte, mit denen Sie die Verwendbarkeit Ihrer UWP-App (App für die Universelle Windows-Plattform) sicherstellen können, wenn ein Design mit hohem Kontrast aktiviert ist."
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: Designs mit hohem Kontrast
label: High-contrast themes
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 50c37d71d3455fc2417d70f04e08a9daff2e881e
ms.openlocfilehash: 4201f5a0b08f1fc8d691218da0803ee04ab2c86a

---

# Designs mit hohem Kontrast  

Beschreibt die Schritte, mit denen Sie die Verwendbarkeit Ihrer UWP-App (App für die Universelle Windows-Plattform) sicherstellen können, wenn ein Design mit hohem Kontrast aktiviert ist.

Eine UWP-App unterstützt standardmäßig Designs mit hohem Kontrast. Wenn sich ein Benutzer für die Verwendung eines Designs mit hohem Kontrast aus den Systemeinstellungen oder Tools für die Barrierefreiheit entscheidet, werden vom Framework automatisch Farben und Stileinstellungen verwendet, mit denen für Steuerelemente und Komponenten auf der Benutzeroberfläche ein Layout und Rendering mit hohem Kontrast entsteht.

Diese standardmäßige Unterstützung basiert auf der Verwendung der Standarddesigns und -vorlagen. Diese Designs und Vorlagen verweisen auf Systemfarben als Ressourcendefinitionen. Die Ressourcenquellen werden automatisch geändert, wenn sich das System in einem Modus mit hohem Kontrast befindet. Wenn Sie jedoch benutzerdefinierte Vorlagen, Designs und Stile für die Steuerelemente verwenden, achten Sie darauf, dass die integrierte Unterstützung des hohen Kontrasts nicht deaktiviert ist. Bei Verwendung eines XAML-Designers für Microsoft Visual Studio für die Formatierung wird vom Designer neben dem primären Design immer ein separates Design mit hohem Kontrast generiert, wenn Sie eine Vorlage definieren, die sich wesentlich von der Standardvorlage unterscheidet. Die separaten Designwörterbücher sind in der [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.themedictionaries.aspx)-Sammlung, einer dedizierten Eigenschaft eines [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794)-Elements, enthalten.

Weitere Informationen zu Designs und Steuerelementvorlagen finden Sie unter [Schnellstart: Steuerelementvorlagen](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374). Die XAML-Ressourcenwörterbücher und -Designs enthalten häufig Informationen zu bestimmten Steuerelementen sowie dazu, wie die Designs konstruiert sind und wie sie auf Ressourcen verweisen, die für die möglichen Einstellungen für hohen Kontrast ähnlich, jedoch nicht identisch sind.

## Designverzeichnisse

Wenn Sie die Systemstandardfarbe ändern oder Bilder wie etwa ein Hintergrundbild zu Dekorationszwecken hinzufügen müssen, erstellen Sie eine **ThemeDictionaries**-Sammlung für Ihre App.

* Erstellen Sie zunächst die richtigen Grundlagen (sofern nicht bereits vorhanden). Erstellen Sie in „App.xaml“ eine **ThemeDictionaries**-Sammlung:

``` xaml
 <Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">

            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">

            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources
```

* 
            **HighContrast** ist nicht der einzige verfügbare Schlüsselname. Es gibt auch noch **HighContrastBlack**, **HighContrastWhite** und **HighContrastCustom**. In den meisten Fällen ist **HighContrast** ausreichend.
* Erstellen Sie unter **Default** den gewünschten [**Brush**](http://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx)-Typ (in der Regel **SolidColorBrush**). Vergeben Sie einen **x:Key**-Namen, der für die Verwendung spezifisch ist:<br/>
    `<SolidColorBrush x:Key="BrandedPageBackground" />`
* Weisen Sie das gewünschte **Color**-Element zu:<br/>
    `<SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />`
* Kopieren Sie das **Brush**-Objekt in **HighContrast**:

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

* Ermitteln Sie die gewünschte Farbe für das **Brush**-Objekt, und ändern Sie sie in **HighContrast**.

Das Festlegen einer Farbe für den hohen Kontrast erfordert etwas Übung. Die zuvor erstellten Grundlagen sind einfach zu aktualisieren.

## Farben mit hohem Kontrast

Benutzer können auf der Einstellungsseite zu hohem Kontrast wechseln. Standardmäßig sind vier Designs mit hohem Kontrast vorhanden. Nachdem der Benutzer eine Option ausgewählt hat, wird auf der Seite eine Vorschau mit der voraussichtlichen Darstellung der Apps angezeigt.

![Einstellungen für hohen Kontrast](images/high-contrast-settings.png)<br/>
_Einstellungen für hohen Kontrast_

 Sie können zum Ändern der Werte auf jedes Quadrat in der Vorschau klicken. Jedes Quadrat ist zudem direkt einer Systemressource zugeordnet.

![Ressourcen mit hohem Kontrast](images/high-contrast-resources.png)<br/>
_Ressourcen mit hohem Kontrast_

Wenn Sie den oben gekennzeichneten Namen das Präfix _SystemColor_ und das Postfix _Color_ hinzufügen, z.B. **SystemColorWindowTextColor**, werden sie automatisch aktualisiert, um der Benutzerangabe zu entsprechen. Dadurch müssen Sie keine bestimmte Farbe für den hohen Kontrast auswählen. Wählen Sie stattdessen eine Systemressource aus, die dem Verwendungszweck der Farbe entspricht. Im oben genannten Beispiel haben wir für die Hintergrundfarbe einer Seite **SolidColorBrushBrandedPageBackground** festgelegt. Da diese Einstellung für einen Hintergrund verwendet wird, können wir sie **SystemColorWindowColor** im Modus mit hohem Kontrast zuordnen:

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Wenn Sie die Palette mit acht Farben mit hohem Kontrast verwenden, müssen Sie keine zusätzlichen **ResourceDictionaries**-Elemente mit hohem Kontrast erstellen. Diese eingeschränkte Palette kann häufig zu Problemen bei der Darstellung komplexer visueller Zustände führen. Das Hinzufügen eines Rahmens nur zu einem Bereich mit hohem Kontrast kann oftmals zu einer besseren Übersichtlichkeit beitragen.

### Empfohlene und nicht empfohlene Vorgehensweisen

* Führen Sie im Modus mit hohem Kontrast frühzeitig und regelmäßig Tests aus.
* Verwenden Sie die benannten Farben für den jeweils beabsichtigten Zweck.
* Platzieren Sie Grundtypen wie **Color**, **Brush** und **Thickness** innerhalb von **ThemeDictionaries**. Platzieren Sie darin keine komplexen Ressourcen wie **Style**-Elemente. Das folgende Beispiel funktioniert einwandfrei:

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>

        <Style x:Key="MyButtonStyle" TargetType="Button">
            <Setter Property="Foreground" Value="{ThemeResource BrandedPageBackground}" />
        </Style>
    </ResourceDictionary>
</Application.Resources>

...

<Button Style="{StaticResource MyButtonStyle}" />
```

* Verwenden Sie für UI-Elemente im Vordergrund Vordergrundfarben mit hohem Kontrast.
* Verwenden Sie Farben mit hohem Kontrast zusammen mit ihren definierten Farbpaaren. Verwenden Sie beispielsweise **BUTTONTEXT** stets mit **BUTTONFACE**, insbesondere bei der Verwendung von Vordergrund und Hintergrund.
* Verwenden Sie für ein bestimmtes UI-Element die empfohlenen Farbpaare für hohen Kontrast, um das erforderliche Kontrastverhältnis von 14:1 zu gewährleisten.
* Trennen Sie Farbpaare für hohen Kontrast nicht, und mischen Sie Farben mit hohem Kontrast nicht beliebig. Dadurch wird häufig nicht sichtbare UI für mindestens eins der vorinstallierten Designs mit hohem Kontrast erzeugt.
* Platzieren Sie keine von Ihnen erstellten **Brush**-Objekte außerhalb einer **ThemeDictionaries**-Sammlung.
* Verwenden Sie niemals **StaticResource** für den Verweis auf eine Ressource in einer **ThemeDictionaries**-Sammlung. Das funktioniert scheinbar, bis der Benutzer bei ausgeführter App Designs ändert. Verwenden Sie stattdessen **ThemeResource**.
* Verwenden Sie keine hartcodierten Farbwerte.
* Verwenden Sie eine Farbe nicht nur deswegen, weil sie Ihnen gefällt.

Weitere Informationen finden Sie unter [XAML-Designressourcen](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources).

## Wann sollten Rahmen verwenden werden?
Fügen Sie im Modus mit hohem Kontrast ggf. Rahmen zu einem UI-Element hinzu, um das Element deutlich abzugrenzen. Verwenden Sie Rahmen, um die Inhaltsbereiche für Navigation, Aktionen und Inhalte voneinander abzugrenzen.

![Ein vom Rest der Seite abgegrenzter Navigationsbereich](images/high-contrast-actions-content.png)<br/>
_Ein vom Rest der Seite abgegrenzter Navigationsbereich_

Wenn ein UI-Element standardmäßig _keinen_ Rahmen oder Hintergrund hat, fügen Sie dem Standardzustand im Modus mit hohem Kontrast keinen Rahmen oder Hintergrund hinzu.

Wenn ein UI-Element standardmäßig über einen Rahmen _verfügt_, behalten Sie den Rahmen im Modus mit hohem Kontrast bei.

Überlappende oder benachbarte Farben sollten unterscheidbar sein. Sie müssen jedoch nicht unbedingt das Farbkontrastverhältnis von 14:1 aufweisen. Für diese Szenarien wird jedoch ein Kontrastverhältnis von 3:1 empfohlen.

Falls Hintergrundfarben mit hohem Kontrast dazu verwendet werden, überlappende UI-Elemente voneinander abzuheben, kann der Kontrast zwischen diesen Elementen nur durch Rahmen gewährleistet werden.

## Ermitteln, wann ein Design mit hohem Kontrast aktiviert ist  
Verwenden Sie Member der [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)-Klasse, um die aktuellen Einstellungen für Designs mit hohem Kontrast zu ermitteln. Mit der [**HighContrast**](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.accessibilitysettings.highcontrast)-Eigenschaft wird ermittelt, ob derzeit ein Design mit hohem Kontrast ausgewählt ist. Falls **true** für **HighContrast** festgelegt ist, besteht der nächste Schritt in der Überprüfung des Werts der [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.accessibilitysettings.highcontrastscheme)-Eigenschaft, um den Namen des verwendeten Designs mit hohem Kontrast abzurufen. „Hoher Kontrast (Weiß)“ und „Hoher Kontrast (Schwarz)“ sind typische Werte für **HighContrastScheme**, auf die Ihr Code reagieren sollte. XAML-definierte [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794)-Schlüssel dürfen keine Leerstellen enthalten. Daher lauten die Schlüssel für diese Designs in einem Ressourcenwörterbuch in der Regel „HighContrastWhite“ und „HighContrastBlack“. Darüber hinaus müssen Sie Fallback-Logik für ein Standarddesign mit hohem Kontrast aufnehmen, falls als Wert eine andere Zeichenfolge angegeben ist. 
            Im [XAML-Beispiel für hohen Kontrast](http://go.microsoft.com/fwlink/p/?linkid=254993) wird die Logik hierfür gezeigt.

> [!NOTE]
> Achten Sie darauf, dass Sie den [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)-Konstruktor aus einem Bereich aufrufen, in dem die App initialisiert ist und bereits Inhalte anzeigt.

Apps können während ihrer Ausführung zur Verwendung von Ressourcenwerten mit hohem Kontrast wechseln. Dies funktioniert, solange die Ressourcen mithilfe der [{ThemeResource}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt185591) im Stil- oder Vorlagen-XAML angefordert werden. Dieses Verfahren der {ThemeResource}-Markuperweiterung wird von allen Standarddesigns (generic.xaml) verwendet. Sie können also dieses Verhalten erzielen, wenn Sie die Standarddesigns für Steuerelemente verwenden. Mit benutzerdefinierten Steuerelementen oder benutzerdefinierten Steuerelementformatierungen ist dies möglich, wenn Sie das Ressourcenverfahren der {ThemeResource}-Markuperweiterung auch in Ihren benutzerdefinierten Vorlagen und Stilen verwendet haben.

## Verwandte Themen  
* [Barrierefreiheit](accessibility.md)
* [Beispiel für UI-Kontrast und -Einstellungen](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML-Beispiel für Barrierefreiheit](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML-Beispiel für hohen Kontrast](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)



<!--HONumber=Jul16_HO1-->


