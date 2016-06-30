---
author: jwmsft
description: "Stellt einen Wert für ein beliebiges XAML-Attribut bereit, indem ein Verweis auf eine Ressource ausgewertet wird. Dabei wird zusätzliche Systemlogik genutzt, mit der unterschiedliche Ressourcen je nach derzeit aktivem Design abgerufen werden."
title: ThemeResource-Markuperweiterung
ms.assetid: 8A1C79D2-9566-44AA-B8E1-CC7ADAD1BCC5
translationtype: Human Translation
ms.sourcegitcommit: 9c657f906e6dedb259b8a98373f56ac5a63bd845
ms.openlocfilehash: 246c991bbdbc95e73ea8d4884cd4d617592bfc51

---

# {ThemeResource}-Markuperweiterung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Stellt einen Wert für ein beliebiges XAML-Attribut bereit, indem ein Verweis auf eine Ressource ausgewertet wird. Dabei wird zusätzliche Systemlogik genutzt, mit der unterschiedliche Ressourcen je nach derzeit aktivem Design abgerufen werden. Ähnlich wie bei der [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md) werden Ressourcen in einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) definiert, und mit einer **ThemeResource**-Syntax wird auf den Schlüssel dieser Ressource im **ResourceDictionary** verwiesen.

## XAML-Attributsyntax

``` syntax
<object property="{ThemeResource key}" .../>
```

## XAML-Werte

| Benennung | Beschreibung |
|------|-------------|
| Schlüssel | Der Schlüssel für die angeforderte Ressource. Dieser Schlüssel wird anfänglich durch das [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) zugewiesen. Ein Ressourcenschlüssel kann eine beliebige in der XamlName-Grammatik definierte Zeichenfolge sein. |
 
## Anmerkungen

Eine **ThemeResource** ist eine Methode zum Abrufen von Werten für ein XAML-Attribut, die an anderer Stelle in einem XAML-Ressourcenwörterbuch definiert sind. Die Markuperweiterung hat den gleichen grundlegenden Zweck wie die [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md). Der Unterschied zum Verhalten gegenüber der {StaticResource}-Markuperweiterung besteht darin, dass von einem **ThemeResource**-Verweis dynamisch verschiedene Wörterbücher als Hauptziel der Suche verwendet werden können, je nachdem, welches Design vom System gerade genutzt wird.

Nach dem ersten Starten der App werden alle Ressourcenverweise eines **ThemeResource**-Verweises anhand des Designs ausgewertet, das beim Starten aktiv war. Wenn Benutzer das aktive Design zur Laufzeit dann jedoch ändern, werden vom System alle **ThemeResource**-Verweise neu ausgewertet und ggf. andere designspezifische Ressourcen abgerufen. Die App wird an allen relevanten Stellen der visuellen Struktur mit neuen Ressourcenwerten angezeigt. Eine **StaticResource** wird zur XAML-Ladezeit bzw. beim Starten der App bestimmt und zur Laufzeit nicht neu ausgewertet. (Es gibt andere Verfahren, z. B. visuelle Zustände, bei denen XAML dynamisch neu geladen wird. Diese Verfahren finden jedoch auf einer höheren Ebene als die grundlegende Ressourcenauswertung statt, die mit der [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md) möglich ist.)

**ThemeResource** akzeptiert ein Argument, das den Schlüssel für die angeforderte Ressource angibt. Ein Ressourcenschlüssel ist immer eine Zeichenfolge in der Windows-Runtime-XAML. Weitere Informationen zum anfänglichen Festlegen des Ressourcenschlüssels finden Sie unter [x:Key-Attribut](x-key-attribute.md).

Weitere Informationen dazu, wie Sie Ressourcen und Eigenschaften mithilfe eines [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) definieren, und zusätzlichen Beispielcode finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](https://msdn.microsoft.com/library/windows/apps/mt187273).

**Wichtig**  
Wie bei **StaticResource** auch darf eine **ThemeResource** nicht versuchen, einen Vorwärtsverweis auf eine Ressource auszuführen, die innerhalb der XAML-Datei weiter lexikalisch definiert ist. Dieser Versuch wird nicht unterstützt. Auch wenn der weitergeleitete Verweis keinen Fehler verursacht, wird durch den Versuch die Leistung beeinträchtigt. Um optimale Ergebnisse zu erzielen, sollten Sie die Ressourcenwörterbücher so erstellen, dass Vorwärtsverweise vermieden werden können.

Wenn Sie versuchen, eine **ThemeResource** für einen Schlüssel anzugeben, die nicht aufgelöst werden kann, führt dies zu einer XAML-Analyseausnahme zur Laufzeit. Entwicklungstools geben unter Umständen auch Warnungen oder Fehler aus.

Die XAML-Prozessorimplementierung der Windows-Runtime enthält keine Sicherungsklassendarstellung für **ThemeResource**-Funktionen. Die weitestgehende Entsprechung im Code ist die Verwendung der Auflistungs-API eines [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794), z. B. der Aufruf von [**Contains**](https://msdn.microsoft.com/library/windows/apps/jj635925) oder [**TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj603139).

**ThemeResource** ist eine Markuperweiterung. Markuperweiterungen werden in der Regel implementiert, wenn für Attributwerte Escapezeichen verwendet werden müssen, damit sie keine Literalwerte oder Handlernamen darstellen, und es nicht ausreicht, Typkonverter für bestimmte Typen oder Eigenschaften zu verwenden. Alle Markuperweiterungen in XAML verwenden die Zeichen „{” und „}” in ihrer Attributsyntax. Anhand dieser Konvention erkennt ein XAML-Prozessor, dass eine Markuperweiterung das Attribut verarbeiten muss.

### Zeitpunkt und Art der Verwendung von {ThemeResource} anstelle von {StaticResource}

Die Regeln, nach denen die Auflösung einer **ThemeResource** zu einem Element in einem Ressourcenwörterbuch erfolgt, sind im Allgemeinen identisch mit den Regeln für **StaticResource**. Eine **ThemeResource**-Suche kann auf die [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Dateien erweitert werden, auf die in einer [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208807)-Auflistung verwiesen wird, aber dies ist auch mit **StaticResource** möglich. Der Unterschied besteht darin, dass eine **ThemeResource** zur Laufzeit neu ausgewertet werden kann, während dies für eine **StaticResource** nicht möglich ist.

Vom Schlüsselsatz in den einzelnen Designverzeichnissen sollte jeweils unabhängig davon, welches Design aktiv ist, der gleiche Ressourcensatz mit Schlüsseln bereitgestellt werden. Wenn eine bestimmte Ressource mit Schlüssel im Designverzeichnis **HighContrast** vorhanden ist, sollte auch in **Light** und **Default** eine Ressource mit diesem Namen vorhanden sein. Anderenfalls tritt bei der Ressourcensuche ggf. ein Fehler auf, wenn Benutzer das Design wechseln. Die Darstellung der App ist dann fehlerhaft. Es ist jedoch möglich, dass ein Designverzeichnis Ressourcen mit Schlüssel enthält, auf die nur innerhalb desselben Bereichs verwiesen wird, um Unterwerte anzugeben. Diese müssen nicht in allen Designs gleich sein.

Generell sollten Sie Ressourcen nur dann in Designverzeichnissen anordnen und mithilfe von **ThemeResource** Verweise auf diese Ressourcen einrichten, wenn sich diese Werte von Design zu Design ändern können oder von sich ändernden Werten unterstützt werden. Dies ist für die folgenden Arten von Ressourcen geeignet:

-   Pinsel, vor allem Farben für [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962). Diese Ressource macht ca. 80 % der Nutzung von **ThemeResource** in den standardmäßigen XAML-Steuerelementvorlagen (generic.xaml) aus.
-   Pixelwerte für Rahmen, Versatz, Ränder und Abstände usw.
-   Schrifteigenschaften wie **FontFamily** oder **FontSize**
-   Vollständige Vorlagen für eine begrenzte Anzahl von Steuerelementen, die normalerweise vom System vorgegeben sind und für die dynamische Darstellung verwendet werden, z. B. [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) und [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919)
-   Textanzeigeformate (etwa zum Ändern von Schriftfarbe, Hintergrund und ggf. der Größe)

Die Windows-Runtime stellt einen Satz von Ressourcen bereit, die ausdrücklich für Verweise über **ThemeResource** gedacht sind. Alle diese Ressourcen werden in der XAML-Datei „themeresources.xaml” aufgelistet, die als Bestandteil des Windows Software Development Kit (SDK) im Ordner „include/winrt/xaml/design” verfügbar ist. Eine Dokumentation zu den Designpinseln und zusätzliche Stile, die in „themeresources.xaml” definiert sind, finden Sie unter [XAML-Designressourcenverweise](https://msdn.microsoft.com/library/windows/apps/mt187274). Die Pinsel sind in einer Tabelle dokumentiert, der Sie den Farbwert eines Pinsels für jedes der drei möglichen aktiven Designs entnehmen können.

Für die XAML-Definitionen von Ansichtszuständen in einer Steuerelementvorlage sollten immer **ThemeResource**-Verweise verwendet werden, wenn eine zugrunde liegende Ressource vorhanden ist, die sich aufgrund einer Designänderung ändern kann. Eine Systemdesignänderung führt normalerweise nicht gleichzeitig auch zu einer Änderung des Ansichtszustands. In diesem Fall müssen für die Ressourcen **ThemeResource**-Verweise verwendet werden, damit Werte für den weiterhin aktiven Ansichtszustand neu ausgewertet werden können. Falls Sie beispielsweise über einen Ansichtszustand verfügen, mit dem eine Pinselfarbe eines bestimmten Teils der UI und eine der dazugehörigen Eigenschaften geändert wird, und diese Pinselfarbe jeweils von Design zu Design unterschiedlich ist, sollten Sie einen **ThemeResource**-Verweis verwenden. Damit stellen Sie den Wert dieser Eigenschaft in der Standardvorlage und außerdem alle Änderungen des Ansichtszustands für diese Standardvorlage bereit.

Die Nutzung von **ThemeResource** lässt sich ggf. in eine Reihe abhängiger Werte erkennen. Beispielsweise kann für einen [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723)-Wert, der von einem [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962)-Element verwendet wird, bei dem es sich außerdem um eine Ressource mit Schlüssel handelt, ein **ThemeResource**-Verweis verwendet werden. Für alle UI-Eigenschaften, von denen die **SolidColorBrush**-Ressource mit Schlüssel verwendet wird, würde jedoch auch ein **ThemeResource**-Verweis verwendet werden. Eine dynamische Änderung des Werts bei einer Designänderung wird also jeweils speziell von den Eigenschaften vom Typ [**Brush**](https://msdn.microsoft.com/library/windows/apps/br228076) ermöglicht.

**Hinweis**
            Die Auswertung von `{ThemeResource}` und Laufzeitressourcen beim Wechseln von Designs wird in Windows 8.1-XAML-Code unterstützt, jedoch nicht in XAML-Code für Apps, die auf Windows 8 ausgerichtet sind.

### Systemressourcen

Von einigen Designressourcen wird auf Systemressourcenwerte als zugrunde liegender Unterwert verwiesen. Eine Systemressource ist eine spezieller Ressourcenwert, der in keinem XAML-Ressourcenverzeichnis zu finden ist. Diese Werte sind vom Verhalten in der XAML-Unterstützung der Windows-Runtime abhängig, um Werte aus dem System selbst weiterzuleiten und in einer Form darzustellen, auf die von einer XAML-Ressource verwiesen werden kann. Beispielsweise gibt es eine Systemressource mit dem Namen „SystemColorButtonFaceColor”, die eine RGB-Farbe darstellt. Diese Farbe stammt von den Aspekten der Systemfarben und -designs, die nicht nur speziell für die Windows-Runtime und Windows-Runtime-Apps gelten.

Systemressourcen sind häufig die zugrunde liegenden Werte für ein Design mit hohem Kontrast. Benutzer können die Farbauswahl für ihr Design mit hohem Kontrast steuern und die Auswahl mithilfe von Systemfeatures treffen, die ebenfalls nicht ausschließlich für Windows-Runtime-Apps gelten. Indem auf die Systemressourcen in Form von **ThemeResource**-Verweisen verwiesen wird, können vom Standardverhalten der Designs mit hohem Kontrast für Windows-Runtime-Apps diese designspezifischen Werte verwendet werden, die vom Benutzer gesteuert und vom System verfügbar gemacht werden. Außerdem sind die Verweise dann für die Neuauswertung gekennzeichnet, wenn vom System während der Laufzeit eine Designänderung erkannt wird.

### {ThemeResource}-Beispielverwendung

Unten ist XAML-Beispielcode aus den Standarddateien „generic.xaml” und „themeresources.xaml” angegeben, um die Verwendung von **ThemeResource** zu veranschaulichen. Wir sehen uns nur eine Vorlage ([**Button**](https://msdn.microsoft.com/library/windows/apps/br209265))-Standardvorlage) und die Deklaration von zwei Eigenschaften ([**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) und [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414)) als Reaktion auf Designänderungen an.

```xml
    <!-- Default style for Windows.UI.Xaml.Controls.Button -->
    <Style TargetType="Button">
        <Setter Property="Background" Value="{ThemeResource ButtonBackgroundThemeBrush}" />
        <Setter Property="Foreground" Value="{ThemeResource ButtonForegroundThemeBrush}"/>
...
```

Für die Eigenschaften wird hier ein [**Brush**](https://msdn.microsoft.com/library/windows/apps/br228076)-Wert verwendet, und der Verweis auf die [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962)-Ressourcen `ButtonBackgroundThemeBrush` und `ButtonForegroundThemeBrush` wird mithilfe von **ThemeResource** durchgeführt.

Diese Eigenschaften werden auch von einigen Ansichtszuständen für ein [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)-Element angepasst. Beispielsweise ändert sich die Hintergrundfarbe, wenn Benutzer auf eine Schaltfläche klicken. Auch hier werden von den Animationen [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) und [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) im Storyboard des Ansichtszustands [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132)-Objekte und Verweise auf Pinsel mit **ThemeResource** als Keyframewert verwendet.

```xml
<VisualState x:Name="Pressed">
  <Storyboard>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Border"
        Storyboard.TargetProperty="Background">
      <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedBackgroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter"
         Storyboard.TargetProperty="Foreground">
       <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedForegroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
  </Storyboard>
</VisualState>
```

Jeder dieser Pinsel wurde bereits in „generic.xaml” definiert: Die Definition musste durchgeführt werden, bevor diese von Vorlagen genutzt wurden, um XAML-Vorwärtsverweise zu vermeiden. Unten sind diese Definitionen für das Designverzeichnis „Default” aufgeführt.

```xml
    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
... 
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="Transparent" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="#FFFFFFFF" />
...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="#FFFFFFFF" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="#FF000000" />
...
```

In allen anderen Designverzeichnissen sind diese Pinsel ebenfalls definiert, z. B.:

```xml
        <ResourceDictionary x:Key="HighContrast">
            <!-- High Contrast theme resources -->
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />

...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
```

Hier ist der [**Color**](https://msdn.microsoft.com/library/windows/apps/br242963) -Wert ein weiterer **ThemeResource**-Verweis auf eine Systemressource. Wenn Sie auf eine Systemressource verweisen und diese als Reaktion auf eine Designänderung ändern möchten, sollten Sie zum Erstellen des Verweises das **ThemeResource**-Element verwenden.

## Windows 8-Verhaltensweise

Unter Windows 8 wurde die **ThemeResource**-Markuperweiterung nicht unterstützt. Sie ist ab Windows 8.1 verfügbar. Außerdem wurde unter Windows 8 auch das dynamische Umschalten zwischen den designbezogenen Ressourcen für eine Windows-Runtime-App nicht unterstützt. Die App musste neu gestartet werden, damit die Designänderung für die XAML-Vorlagen und -Formate wirksam wurde. Dieses Verhalten beeinträchtigt die Benutzerfreundlichkeit. Es wird daher dringend empfohlen, Apps neu zu kompilieren und auf Windows 8.1 auszurichten, damit Formate mit Nutzung von **ThemeResource** verwendet werden können und dynamisch zwischen Designs gewechselt werden kann, wenn Benutzer dies wünschen. Apps, die für Windows 8 kompiliert wurden, aber unter Windows 8.1 ausgeführt werden, weisen weiterhin das Windows 8-Verhalten auf.

## Unterstützung von Entwurfszeittools für die **{ThemeResource}**-Markuperweiterung

Microsoft Visual Studio 2013 kann mögliche Schlüsselwerte in die Microsoft IntelliSense-Dropdownelemente einbinden, wenn Sie die **{ThemeResource}**-Markuperweiterung auf einer XAML-Seite verwenden. Sobald Sie z. B. „{ThemeResource” eingeben, wird ein beliebiger Ressourcenschlüssel aus den [XAML-Designressourcen](https://msdn.microsoft.com/library/windows/apps/mt187274) angezeigt.

Sobald ein Ressourcenschlüssel als Teil einer **{ThemeResource}**-Verwendung vorhanden ist, kann das Feature **Gehe zu Definition** (F12) diese Ressource auflösen und Ihnen die Datei „generic.xaml” für die Entwurfszeit anzeigen, in der die Designressource definiert ist. Da Designressourcen öfter definiert werden (eine pro Design), leitet **Gehe zu Definition** Sie zu der ersten Definition weiter, die in der Datei gefunden wird; dabei handelt es sich um die Definition für **Default**. Sollten Sie die anderen Definitionen benötigen, können Sie innerhalb der Datei anhand des Schlüsselnamens nach den Definitionen der anderen Designs suchen.

## Verwandte Themen

* [ResourceDictionary- und XAML-Ressourcenreferenzen](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [XAML-Designressourcen](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [**Ressourcenwörterbuch**](https://msdn.microsoft.com/library/windows/apps/br208794)
* [x:Key-Attribut](x-key-attribute.md)
 




<!--HONumber=Jun16_HO4-->


