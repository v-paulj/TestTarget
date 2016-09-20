---
author: jwmsft
description: "Sie lernen die XAML-Sprache und XAML-Konzepte für die Entwicklergruppe von Windows-Runtime-Apps kennen. Darüber hinaus werden die verschiedenen Methoden zum Deklarieren von Objekten und Festlegen von Attributen in XAML beim Erstellen einer Windows-Runtime-App erläutert."
title: "Übersicht über XAML"
ms.assetid: 48041B37-F1A8-44A4-BB8E-1D4DE30E7823
ms.sourcegitcommit: 57b406f8210a9de729deec1fd2003973ac91f9cd
ms.openlocfilehash: 9ddb584efe7c6406f78b5a0cf0bdc73a974afd18

---

# Übersicht über XAML

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Sie lernen die XAML-Sprache und XAML-Konzepte für die Entwicklergruppe von Windows-Runtime-Apps kennen. Darüber hinaus werden die verschiedenen Methoden zum Deklarieren von Objekten und Festlegen von Attributen in XAML beim Erstellen einer Windows-Runtime-App erläutert.

## Was ist XAML?

XAML (Extensible Application Markup Language) ist eine deklarative Programmiersprache. Insbesondere können mit XAML Objekte initialisiert und Eigenschaften von Objekten festgelegt werden. Dazu wird eine Sprachstruktur verwendet, die hierarchische Beziehungen zwischen mehreren Objekten anzeigt, und eine Unterstützungstypkonvention, die die Erweiterung von Typen unterstützt. Sie können sichtbare UI-Elemente im deklarativen XAML-Markup erstellen. Anschließend können Sie eine separate CodeBehind-Datei für jede XAML-Datei zuordnen, die auf Ereignisse reagieren und die ursprünglich in XAML deklarierten Objekte verändern kann.

Die XAML-Sprache unterstützt den Austausch von Quellen zwischen unterschiedlichen Tools und Rollen für den Entwicklungsprozess, z.B. das Austauschen von XAML-Quellen zwischen Designtools und einer IDE oder zwischen Primärentwicklern und Lokalisierungsentwicklern. Durch die Verwendung von XAML als Austauschformat können Designerrollen und Entwicklerrollen separat gehalten oder zusammengeführt und von Designern und Entwicklern während der Produktion einer App durchlaufen werden.

Im Rahmen Ihrer Windows-Runtime-App-Projekte handelt es sich bei den XAML-Dateien um XML-Dateien mit der Dateinamenerweiterung XAML.

## Grundlegende XAML-Syntax

XAML verfügt über eine grundlegende Syntax, die auf dem XML-Format basiert. Der Definition nach muss es sich bei gültigen XAML-Daten auch um gültige XML-Daten handeln. XAML verfügt jedoch auch über Syntaxkonzepte, denen eine unterschiedliche und ausführlichere Bedeutung zugewiesen wurde, bei denen es sich gleichzeitig jedoch um gültige XML-Daten gemäß XML1.0-Spezifikation handelt. Beispielsweise unterstützt XAML die *Syntax von Eigenschaftselementen*, in der Eigenschaftswerte innerhalb von Elementen festgelegt werden können, anstatt über Zeichenfolgenwerte in Attributen oder Inhalten. Für das normale XML-Format ist ein XAML-Eigenschaftselement ein Element mit einem Punkt im Namen, damit es für einfaches XML gültig ist, jedoch nicht die gleiche Bedeutung hat.

## XAML und Microsoft Visual Studio

Microsoft Visual Studio erleichtert das Erstellen gültiger XAML-Syntax, sowohl im XAML-Text-Editor als auch auf der eher grafikorientierten XAML-Designoberfläche. Machen Sie sich beim Schreiben von XAML-Code für die App mit Visual Studio also nicht bei jedem Tastenanschlag unnötig viele Sorgen um die Syntax. In der Entwicklungsumgebung erhalten Sie Unterstützung beim Schreiben gültiger XAML-Syntax, indem die Autovervollständigung verwendet wird und Vorschläge in Microsoft IntelliSense-Listen und -Dropdownlisten angezeigt, UI-Elementbibliotheken in der Toolbox angegeben oder andere Methoden genutzt werden. Falls Sie das erste Mal mit XAML arbeiten, kann es jedoch durchaus nützlich sein, sich mit den Syntaxregeln und insbesondere der zum Beschreiben der Beschränkungen oder Auswahlmöglichkeiten verwendeten Terminologie vertraut zu machen. Beachten Sie besonders die Abschnitte, in denen die XAML-Syntax in Referenzthemen oder anderen Themen beschrieben wird. Auf diese Einzelheiten der XAML-Syntax gehen wir im separaten Artikel [Anleitung zur XAML-Syntax](xaml-syntax-guide.md) ein.

## XAML-Namespaces

Bei der allgemeinen Programmierung handelt es sich bei einem Namespace um ein Organisationskonzept, mit dem festgelegt wird, wie Bezeichner für Programmierentitäten interpretiert werden. Wenn Sie Namespaces verwenden, kann ein Programmierframework benutzerdeklarierte von frameworkdeklarierten Bezeichnern unterscheiden, Mehrdeutigkeiten bei Bezeichnern durch Namespacequalifizierung auflösen und Regeln für die Bereichsdefinition von Namen durchsetzen usw. XAML verfügt in Bezug auf die XAML-Sprache über ein entsprechendes eigenes XAML-Namespace-Konzept. Im Folgenden wird erklärt, wie XAML die Konzepte für den XML-Sprachnamespace anwendet und erweitert:

-   Für XAML wird das reservierte XML-Attribut **xmlns** für Namespacedeklarationen verwendet. Der Wert des Attributs ist normalerweise ein Uniform Resource Identifier (URI). Dies ist eine von XML geerbte Konvention.
-   XAML nutzt Präfixdeklarationen, um nicht standardmäßige Namespaces zu deklarieren, und außerdem Präfixverwendungen in Elementen und Attributen, um auf den Namespace zu verweisen.
-   Für XAML wird ein Standardnamespace verwendet, der als Namespace eingesetzt wird, wenn bei einer Nutzung oder Deklaration kein Präfix vorhanden ist. Der Standardnamespace kann für jedes XAML-Programmierframework unterschiedlich definiert werden.
-   Namespacedefinitionen erben in einer XAML-Datei oder einem -Konstrukt vom übergeordneten zum untergeordneten Element. Wenn Sie beispielsweise im Stammelement einer XAML-Datei einen Namespace definieren, übernehmen alle Elemente innerhalb dieser Datei die Namespacedefinition. Wenn ein Element weiter unten auf der Seite den Namespace neu definiert, übernehmen die Nachfolgerelemente dieses Elements die neue Definition.
-   Attribute eines Elements erben dessen Namespaces. Es ist unüblich, XAML-Attribute mit Präfixen zu verwenden.

Eine XAML-Datei deklariert fast immer einen Standard-XAML-Namespace im Stammelement. Der Standard-XAML-Namespace definiert, welche Elemente deklariert werden können, ohne sie durch ein Präfix zu qualifizieren. Bei typischen Windows-Runtime-App-Projekten enthält dieser Standardnamespace das gesamte integrierte XAML-Vokabular für die Windows-Runtime zur Definition der UI: Standardsteuerelemente, Textelemente, XAML-Grafiken und -Animationen, Datenbindungs- und Formatierungsunterstützungstypen usw. Beim Schreiben von XAML-Code für Windows-Runtime-Apps können Sie die Verwendung von XAML-Namespaces und -Präfixen also weitestgehend vermeiden, wenn Sie auf häufig genutzte UI-Elemente verweisen.

Unten ist ein Codeausschnitt mit einem über eine Vorlage erstellten [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)-Stamm der Startseite für eine App angegeben (nur mit Starttag und vereinfacht). Darin werden der Standardnamespace und der **x** -Namespace deklariert (wird als Nächstes erklärt).

```xml
<Page
    x:Class="Application1.BlankPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
>
```

## XAML-Namespace der XAML-Sprache

Ein spezieller XAML-Namespace, der in fast jeder Windows-Runtime-XAML-Datei deklariert ist, ist der XAML-Sprachnamespace. Dieser Namespace enthält Elemente und Konzepte, die durch die XAML-Sprache (Sprachspezifikation) definiert sind. Üblicherweise wird der XAML-Namespace für die XAML-Sprache dem Präfix „x“ zugeordnet. Die standardmäßigen Projekt- und Dateivorlagen für Projekte für Windows-Runtime-Apps definieren immer sowohl den Standard-XAML-Namespace (kein Präfix, nur `xmlns=`) als auch den XAML-Sprachnamespace (Präfix „x“) als Teil des Stammelements.

Der XAML-Sprachnamespace mit dem Präfix „x“ enthält mehrere Programmierkonstrukte, die in XAML häufig verwendet werden. Die häufigsten lauten:

| Benennung | Beschreibung |
|------|-------------|
| [x:Key](x-key-attribute.md) | Legt einen eindeutigen benutzerdefinierten Schlüssel für jede Ressource in einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-XAML-Element fest. Die Schlüsseltoken-Zeichenfolge ist das Argument für die **StaticResource**-Markuperweiterung. Sie verwenden diesen Schlüssel später zum Abrufen der XAML-Ressourcen aus einer anderen XAML-Verwendung in der XAML Ihrer App. |
| [x:Class](x-class-attribute.md) | Gibt den Code-Namespace und Codeklassennamen für die Klasse an, mit der CodeBehind-Daten für eine XAML-Seite bereitgestellt werden. Damit wird die Klasse benannt, die beim Erstellen Ihrer App erstellt oder zugeordnet wird. Diese Buildvorgänge unterstützen die XAML-Markupkompilierung und kombinieren das Markup und CodeBehind, wenn die App kompiliert wird. Eine solche Klasse ist für die Unterstützung von CodeBehind für eine XAML-Seite erforderlich. 
            [
              **Window.Content**
            ](https://msdn.microsoft.com/library/windows/apps/br209051) im Windows-Runtime-Aktivierungs-Standardmodell. |
| [x:Name](x-name-attribute.md) | Gibt einen Laufzeitobjektnamen für die Instanz in Laufzeitcode an, nachdem ein in XAML definiertes Objektelement verarbeitet wird. Das Festlegen von **x:Name** in XAML ist mit dem Deklarieren einer benannten Variable in Code vergleichbar. Wie Sie später erfahren werden, geschieht genau das, wenn XAML als Komponente einer Windows-Runtime-App geladen wird. <br/><div class="alert">
            **Hinweis**
            [
              **FrameworkElement.Name**
            ](https://msdn.microsoft.com/library/windows/apps/br208735) ist eine ähnliche Eigenschaft im Framework, die jedoch nicht von allen Elementen unterstützt wird. Sie verwenden also **x:Name** für die Elementidentifikation, wenn **FrameworkElement.Name** für den Elementtyp nicht unterstützt wird. |
| [x:Uid](x-uid-directive.md) | Bezeichnet Elemente, die für einige ihrer Eigenschaftswerte lokalisierte Ressourcen verwenden sollen. Weitere Informationen zur Verwendung von **x:Uid** finden Sie unter [Schnellstart: Übersetzen von UI-Ressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329). |
| [Systeminterne XAML-Datentypen](xaml-intrinsic-data-types.md) | Diese Typen können Werte für einfache Werttypen angeben, wenn dies für ein Attribut oder eine Ressource erforderlich ist. Diese systeminternen Typen entsprechen den einfachen Werttypen, die normalerweise als Teil der systeminternen Definitionen der jeweiligen Programmiersprache definiert sind. So benötigen Sie unter Umständen ein Objekt, das den booleschen Wert **true** darstellt, damit dieser in einem visuellen Storyboardzustand von [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320) verwendet werden kann. Für diesen Wert in XAML würden Sie den systeminternen Typ **x:Boolean** folgendermaßen als Objektelement verwenden: <code>&lt;x:Boolean&gt;True&lt;/x:Boolean&gt;</code> | 

Es werden auch andere Programmierkonstrukte im XAML-Sprachnamespace verwendet. Diese sind jedoch weniger gebräuchlich.

## Zuordnen von benutzerdefinierten Typen zu XAML-Namespaces

Einer der nützlichsten Aspekte von XAML als Sprache besteht darin, dass die Erweiterung des XAML-Vokabulars für Windows-Runtime-Apps einfach ist. Definieren Sie dazu in der Programmiersprache der App Ihre eigenen benutzerdefinierten Typen, und verweisen Sie im XAML-Markup dann auf Ihre benutzerdefinierten Typen. Die Unterstützung für die Erweiterung mithilfe von benutzerdefinierten Typen ist integraler Bestandteil der Funktionsweise der XAML-Sprache. Frameworks oder App-Entwickler sind für das Erstellen der Sicherungsobjekte zuständig, auf die XAML verweist. Weder Frameworks noch der App-Entwickler sind über die grundlegenden XAML-Syntaxregeln hinaus in Bezug auf die Rolle oder Funktion der Objekte in den Vokabularen an Spezifikationen gebunden (es bestehen einige Erwartungen an die Funktionen der XAML-Sprachen und XAML-Namespacetypen, die Windows-Runtime stellt jedoch die erforderliche Unterstützung bereit).

Wenn Sie XAML für Typen verwenden, die aus anderen Bibliotheken als den Windows Runtime-Kernbibliotheken und -Metadaten stammen, müssen Sie einen XAML-Namespace deklarieren und diesem ein Präfix zuordnen. Verwenden Sie dieses Präfix in Elementnutzungen, um auf die in Ihrer Bibliothek definierten Typen zu verweisen. Sie deklarieren Präfixzuordnungen als **xmlns**-Attribute, und zwar meist in einem Stammelement zusammen mit den anderen XAML-Namespacedefinitionen.

Zum Erstellen Ihrer eigenen Namespacedefinition, mit der auf benutzerdefinierte Typen verwiesen wird, geben Sie zuerst das Schlüsselwort **xmlns:** und dann das gewünschte Präfix an. Der Wert des Attributs muss das Schlüsselwort **using:** als ersten Teil des Werts enthalten. Der restliche Wert ist ein Zeichenfolgentoken, mit dem der Namespace für die Codesicherung, der Ihre benutzerdefinierten Typen enthält, anhand des Namens referenziert wird.

Das Präfix definiert das Markuptoken, das zur Referenzierung dieses XAML-Namespaces im Rest des Markups der XAML-Datei verwendet wird. Ein Doppelpunkt (":") wird zwischen das Präfix und die im XAML-Namespace zu referenzierende Entität gesetzt.

Die Attributsyntax zum Zuordnen des Präfix `myTypes` zum Namespace `myCompany.myTypes` lautet z.B. wie folgt: `    xmlns:myTypes="using:myCompany.myTypes"`. Ein Beispiel für eine Elementdarstellung lautet: `<myTypes:CustomButton/>`

Weitere Informationen zur Zuordnung von XAML-Namespaces für benutzerdefinierte Typen einschließlich einiger besonderer Punkte zu Visual C++-Komponentenerweiterungen (C++/CX) finden Sie unter [XAML-Namespaces und -Namespacezuordnung](xaml-namespaces-and-namespace-mapping.md).

## Andere XAML-Namespaces

Häufig sehen Sie XAML-Dateien, die die Präfixe „d“ (für Designernamespace) und „mc“ (zur Markupkompatibilität) definieren. Im Allgemeinen dienen diese der Infrastrukturunterstützung oder zum Aktivieren von Szenarien in einem Entwurfszeittool. Weitere Informationen finden Sie im [Abschnitt "Andere XAML-Namespaces" des Themas zu XAML-Namespaces](xaml-namespaces-and-namespace-mapping.md#other-XAML-namespaces).

## Markuperweiterungen

Markuperweiterungen sind ein XAML-Sprachkonzept, das häufig bei der Windows-Runtime-XAML-Implementierung verwendet wird. Häufig stellen Markuperweiterungen eine Art "Verknüpfung" dar, mit deren Hilfe eine XAML-Datei auf einen Wert oder ein Verhalten zugreifen kann, bei dem Elemente nicht einfach nur anhand von Sicherungstypen deklariert werden. Einige Markuperweiterungen können Eigenschaften mit einfachen Zeichenfolgen oder zusätzlichen geschachtelten Elementen festlegen; dies dient der Optimierung der Syntax bzw. der Faktoren zwischen verschiedenen XAML-Dateien.

In der XAML-Attributsyntax zeigen die geschweiften Klammern „{“ und „}“ die Verwendung von XAML-Markuperweiterungen an. Die Verwendung weist die XAML-Verarbeitung an, die allgemeine Behandlung von Attributwerten entweder als Literalzeichenfolge oder direkten zeichenfolgenkonvertiblen Wert zu vermeiden. Stattdessen ruft ein XAML-Parser Code auf, der das Verhalten für die jeweilige Markuperweiterung bereitstellt, und dieser Code stellt ein alternatives Ergebnis für das Objekt oder Verhalten bereit, dass der XAML-Parser benötigt. Markuperweiterungen können über Argumente verfügen, die dem Namen der Markuperweiterung folgen und auch innerhalb der geschweiften Klammern enthalten sind. Normalerweise stellt eine ausgewertete Markuperweiterung einen Objektrückgabewert bereit. Bei der Analyse wird dieser Rückgabewert an der Stelle in der Objektstruktur eingefügt, an der sich die Verwendung der Markuperweiterung in der XAML-Quelle befand.

Windows-Runtime-XAML unterstützt diese Markuperweiterungen, die unter dem Standard-XAML-Namespace definiert und vom Windows-Runtime-XAML-Parser verstanden werden:

-   
            [{xBind}](x-bind-markup-extension.md): unterstützt die Datenbindung. Dadurch wird die Eigenschaftenauswertung durch Ausführung eines speziellen zur Kompilierungszeit generierten Codes bis zur Laufzeit zurückgestellt. Diese Markuperweiterung unterstützt einen weiten Bereich von Argumenten.
-   
            [{Binding}](binding-markup-extension.md): unterstützt die Datenbindung. Dadurch wird die Eigenschaftenauswertung durch Ausführung einer allgemeinen Laufzeitobjektüberprüfung bis zur Laufzeit zurückgestellt. Diese Markuperweiterung unterstützt einen weiten Bereich von Argumenten.
-   
            [{StaticResource}](staticresource-markup-extension.md): unterstützt verweisende Ressourcenwerte, die in [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) definiert werden. Diese Ressourcen können sich in einer anderen XAML-Datei befinden, müssen vom XAML-Parser zur Ladezeit jedoch auffindbar sein. Mit dem Argument der Verwendung von `{StaticResource}` wird der Schlüssel (der Name) für eine Ressource mit Schlüssel in einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) identifiziert.
-   
            [{ThemeResource}](themeresource-markup-extension.md): ähnlich wie die [{StaticResource}](staticresource-markup-extension.md)-Markuperweiterung, kann jedoch auf Designänderungen während der Laufzeit reagieren. {ThemeResource} tritt bei den Windows-Runtime-XAML-Standardvorlagen relativ häufig auf, da die meisten dieser Vorlagen darauf ausgerichtet sind, dass der Benutzer das Design wechseln kann, während die App ausgeführt wird.
-   
            [{TemplateBinding}](templatebinding-markup-extension.md): ein Sonderfall von [{Binding}](binding-markup-extension.md), bei dem Steuerelementvorlagen in XAML und deren Nutzung zur Laufzeit unterstützt werden.
-   
            [{RelativeSource}](relativesource-markup-extension.md): Ermöglicht eine bestimmte Form der Vorlagenbindung, bei der die Werte aus dem vorlagenbasierten übergeordneten Element stammen.
-   
            [{CustomResource}](customresource-markup-extension.md): Für erweiterte Ressourcensuchszenarien.

Die Windows-Runtime unterstützt außerdem die [{x:Null}-Markuperweiterung](x-null-markup-extension.md). Diese wird verwendet, um [**Nullable**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)-Werte in XAML auf **null** festzulegen. Beispielsweise können Sie diese Markuperweiterung in einer Steuerelementvorlage für ein [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)-Element verwenden, bei der **null** als unbestimmter Überprüfungsstatus interpretiert wird (Auslösung des Darstellungszustands „Unbestimmt“).

Eine Markuperweiterung gibt im Allgemeinen eine vorhandene Instanz aus einem anderen Teil des Objektdiagramms für die App zurück oder stellt einen Wert zur Laufzeit zurück. Da Sie eine Markuperweiterung als Attributwert verwenden können und dies auch die übliche Art der Nutzung ist, ist häufig zu beobachten, dass Markuperweiterungen Werte für Eigenschaften vom Typ "Verweis" bereitstellen, für die andernfalls eine Eigenschaftselementsyntax erforderlich gewesen wäre.

Die Syntax zum Verweisen auf ein wiederverwendbares [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)-Element aus einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) lautet z.B. wie folgt: `<Button Style="{StaticResource SearchButtonStyle}"/>`. Ein [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)-Element ist ein Verweistyp, kein einfacher Wert. Ohne `{StaticResource}` würden Sie darin also ein `<Button.Style>`-Eigenschaftselement und eine `<Style>`-Definition benötigen, um die [**FrameworkElement.Style**](https://msdn.microsoft.com/library/windows/apps/br208743)-Eigenschaft festzulegen.

Bei Einsatz von Markuperweiterungen kann jede Eigenschaft, die unter XAML festgelegt werden kann, praktisch auch in Attributsyntax festgelegt werden. Mithilfe der Attributsyntax können Sie auch dann Verweiswerte für eine Eigenschaft bereitstellen, wenn diese ansonsten keine Attributsyntax für die direkte Objektinstanziierung unterstützt. Oder Sie können ein spezifisches Verhalten aktivieren, das die allgemeine Anforderung zurückstellt, dass XAML-Eigenschaften durch Wertetypen oder neu erstellte Verweistypen gefüllt werden.

Um dies zu veranschaulichen, wird im nächsten XAML-Beispiel der Wert der [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743)-Eigenschaft von [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) mithilfe der Attributsyntax festgelegt. Die [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743)-Eigenschaft akzeptiert eine Instanz der [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)-Klasse. Dies ist ein Verweistyp, der standardmäßig nicht mithilfe einer Attributsyntaxzeichenfolge erstellt werden konnte. In diesem Fall verweist jedoch das Attribut auf eine bestimmte Markuperweiterung, [StaticResource](staticresource-markup-extension.md). Wenn diese Markuperweiterung verarbeitet wird, gibt sie einen Verweis auf ein **Style**-Element zurück, das zuvor als Ressource mit Schlüssel in einem Ressourcenverzeichnis definiert wurde.

```xml
<Canvas.Resources>
  <Style TargetType="Border" x:Key="PageBackground">
    <Setter Property="BorderBrush" Value="Blue"/>
    <Setter Property="BorderThickness" Value="5"/>
  </Style>
</Canvas.Resources>
...
<Border Style="{StaticResource PageBackground}">
  ...
</Border>
```

Markuperweiterungen können geschachtelt werden. Die innerste Markuperweiterung wird zuerst ausgewertet.

Aufgrund der Markuperweiterungen benötigen Sie für einen Literalwert „{“ in einem Attribut eine spezielle Syntax. Weitere Informationen finden Sie unter [Anleitung zur XAML-Syntax](xaml-syntax-guide.md).

## Ereignisse

XAML ist eine deklarative Programmiersprache für Objekte und ihre Eigenschaften, sie enthält jedoch auch eine Syntax zum Anfügen von Ereignishandlern an Objekte im Markup. Die XAML-Ereignissyntax kann dann die XAML-deklarierten Ereignisse über das Windows-Runtime-Programmiermodell integrieren. Sie geben den Namen des Ereignisses als Attributnamen in dem Objekt an, in dem das Ereignis behandelt wird. Für den Attributwert geben Sie den Namen einer Ereignishandler-Funktion an, die Sie im Code definieren. Der XAML-Prozessor verwendet diesen Namen zum Erstellen einer Delegatdarstellung in der geladenen Objektstruktur und fügt den angegebenen Handler einer internen Handlerliste hinzu. Fast alle Windows-Runtime-Apps werden sowohl durch Markup- als auch durch CodeBehind-Quellen definiert.

Dies ist ein einfaches Beispiel. Die [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)-Klasse unterstützt ein Ereignis mit dem Namen [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737). Sie können einen Handler für **Click** schreiben, mit dem Code ausgeführt wird, der aufgerufen wird, nachdem Benutzer auf das **Button**-Element geklickt haben. Im XAML-Code geben Sie **Click** als Attribut des **Button**-Elements an. Stellen Sie für den Attributwert eine Zeichenfolge bereit, bei der es sich um den Methodennamen des Handlers handelt.

```xml
<Button Click="showUpdatesButton-Click">Show updates</Button>
```

Bei der Kompilierung wird vom Compiler dann erwartet, dass eine Methode mit dem Namen `showUpdatesButton-Click` in der CodeBehind-Datei und im Namespace definiert ist, der im [x:Class](x-class-attribute.md)-Wert der XAML-Seite deklariert wurde. Mit dieser Methode muss außerdem der Delegatvertrag für das [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737)-Ereignis erfüllt werden. Beispiel:

> [!div class="tabbedCodeSnippets"]
```csharp
namespace App1
{
    public sealed partial class MainPage: Page {
        ...
        private void showUpdatesButton-Click (object sender, RoutedEventArgs e) {
            //your code
        }
    }
}
```
```vb
' Namespace included at project level
Public NotInheritable Class MainPage
    Inherits Page
        ...
        Private Sub showUpdatesButton-Click (sender As Object, e As RoutedEventArgs e)
            ' your code
        End Sub
    ...
End Class
```
```cpp
// .h
namespace App1
{
    public ref class MainPage sealed {
        ...
    private:
        void showUpdatesButton-Click (Object^ sender, RoutedEventArgs^ e);
    };
}
```

[!div class="tabbedCodeSnippets"] Innerhalb eines Projekts wird XAML als xaml-Datei geschrieben, und Sie können mit Ihrer bevorzugten Programmiersprache (C#, Visual Basic, C++/CX) eine CodeBehind-Datei schreiben. Wenn für eine XAML-Datei als Teil einer Buildaktion für das Projekt Markup kompiliert wird, wird die Position der XAML-CodeBehind-Datei für jede XAML-Seite durch Angabe eines Namespaces und einer Klasse als [x:Class](x-class-attribute.md)-Attribut des Stammelements der XAML-Seite bestimmt.

Weitere Informationen zur Funktionsweise dieser Mechanismen in XAML und ihrer Beziehung zu den Programmierungs- und Anwendungsmodellen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](events-and-routed-events-overview.md). 
            **Hinweis:** Für C++/CX gibt es zwei CodeBehind-Dateien: eine für einen Header (.xaml.h) und eine für die Implementierung (.xaml.cpp).

## Die Implementierung verweist auf den Header, und aus technischer Sicht ist es der Header, der den Einstiegspunkt für die CodeBehind-Verbindung darstellt.

Ressourcenwörterbücher Die Erstellung eines [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Elements ist eine häufige Aufgabe. Dabei wird ein Ressourcenwörterbuch normalerweise als Bereich einer XAML-Seite oder als separate XAML-Datei erstellt. Ressourcenwörterbücher und ihre Verwendung sind ein umfangreicher Bereich, dessen Erläuterung den Rahmen dieses Themas sprengen würde.

## Weitere Informationen finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](https://msdn.microsoft.com/library/windows/apps/mt187273).

XAML und XML Die XAML-Sprache basiert im Großen und Ganzen auf der XML-Sprache. Durch XAML wird XML jedoch bedeutend erweitert. Im Speziellen das Schemakonzept wird wegen der Beziehung zum Unterstützungstyp anders behandelt, und es werden Sprachelemente wie angefügte Member und Markuperweiterungen hinzugefügt. 
            **xml:lang** ist in XAML gültig, beeinflusst jedoch anstelle des Analyseverhaltens das Laufzeitverhalten, und der Alias ist normalerweise einer Eigenschaft auf Frameworkebene zugeordnet. Weitere Informationen finden Sie unter [**FrameworkElement.Language**](https://msdn.microsoft.com/library/windows/apps/hh702066). 
            **xml:base** ist im Markup gültig, wird aber von Parsern ignoriert. 
            **xml:space** ist gültig, jedoch nur für im Thema [XAML und Leerzeichen](xaml-and-whitespace.md) beschriebene Szenarien relevant. Das **encoding**-Attribut ist in XAML gültig. Nur UTF-8- und UTF-16-Codierungen werden unterstützt.

###  UTF-32-Codierung wird nicht unterstützt.

Unterscheidung nach Groß-/Kleinschreibung in XAML In XAML wird die Groß-/Kleinschreibung berücksichtigt. Dies ist eine weitere Konsequenz daraus, dass XAML auf XML basiert, in der die Groß-/Kleinschreibung berücksichtigt wird. In Namen von XAML-Elementen und -Attributen wird die Groß-/Kleinschreibung berücksichtigt. Im Wert eines Attributs wird die Groß-/Kleinschreibung unter Umständen berücksichtigt; dies hängt davon ab, wie der Attributwert für bestimmte Eigenschaften behandelt wird. Wenn der Attributwert beispielsweise einen Membernamen einer Enumeration deklariert, wird für das integrierte Verhalten, das eine Membernamen-Zeichenfolge typkonvertiert, um den Enumerationsmemberwert zurückzugeben, die Groß-/Kleinschreibung nicht berücksichtigt.

## Im Gegensatz dazu behandeln der Wert der **Name**-Eigenschaft und Hilfsmethoden zum Arbeiten mit Objekten basierend auf dem Namen, den die **Name**-Eigenschaft deklariert, die Namenszeichenfolge unter Berücksichtigung der Groß-/Kleinschreibung.

XAML-NameScopes Die XAML-Sprache definiert ein Konzept eines XAML-NameScopes. Das XAML-NameScope-Konzept beeinflusst, wie XAML-Prozessoren den Wert von **x:Name** oder **Name**, angewendet auf XAML-Elemente, behandeln, insbesondere die Bereiche, in denen Namen anwendbare eindeutige IDs sein sollten.

## XAML-NameScopes werden ausführlicher im separaten Thema [XAML-NameScopes](xaml-namescopes.md) behandelt.

Die Rolle von XAML im Entwicklungsprozess

-   XAML spielt in vielerlei Hinsicht eine wichtige Rolle beim App-Entwicklungsprozess. XAML stellt das Hauptformat für die Deklarierung der Benutzeroberfläche und deren Elementen in einer App dar, sofern deren Programmierung mit C#, Visual Basic oder C++/CX erfolgt. In der Regel stellt mindestens eine XAML-Datei in Ihrem Projekt eine Seitenmetapher in Ihrer App für die zu Anfang angezeigte Benutzeroberfläche dar. Zusätzliche XAML-Dateien können weitere Seiten für die Navigationsoberfläche deklarieren.
-   Andere XAML-Dateien können Ressourcen deklarieren, etwa Vorlagen und Stile.
-   Das XAML-Format wird zur Deklaration von Stilen und Vorlagen genutzt, die in einer App auf Steuerelemente und die Benutzeroberfläche angewendet werden. Dabei können Stile und Vorlagen entweder für bestehende Steuerelemente in Vorlagen überführt oder ein Steuerelement definiert werden, das Standardvorlagen als Teil eines Steuerelementepakets bereitstellt.
-   Wenn es dazu verwendet wird, Stile und Vorlagen zu definieren, wird der relevante XAML-Code oft als eigenständige XAML-Datei mit einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Stamm deklariert. XAML ist das gemeinsame Format für Designerunterstützung, mit der Benutzeroberflächen für Apps erstellt und das Design zwischen verschiedenen Designer-Apps ausgetauscht werden kann.
-   Am wichtigsten dürfte hier die Möglichkeit sein, den XAML-Code für die App zwischen unterschiedlichen XAML-Designtools (oder Designfenstern in Tools) austauschen zu können. Auch viele andere Technologien definieren die grundlegende Benutzeroberfläche in XAML. In Bezug auf das Windows Presentation Foundation (WPF)- und Microsoft Silverlight-XAML-Format nutzt das XAML-Format für Windows-Runtime denselben URI für seinen gemeinsam verwendeten Standard-XAML-Namespace. Das XAML-Vokabular für Windows-Runtime überschneidet sich stark mit dem XAML-Vokabular für Benutzeroberflächen, das ebenfalls von Silverlight und zu einem etwas geringeren Grad von WPF genutzt wird.
-   Damit unterstützt XAML eine effiziente Migrationsmöglichkeit für Benutzeroberflächen, die ursprünglich für Vorgängertechnologien definiert wurden und ebenfalls auf XAML gesetzt haben. XAML definiert die visuelle Darstellung einer Benutzeroberfläche, und eine zugehörige CodeBehind-Datei definiert die Logik. Das Oberflächendesign kann geändert werden, ohne Änderungen an der Logik im CodeBehind vornehmen zu müssen.
-   XAML vereinfacht den Workflow zwischen Designern und Entwicklern.

Aufgrund der umfangreichen Unterstützung des visuellen Designers und der Designoberflächen für die XAML-Sprache, unterstützt XAML Rapid-UI-Prototyping in frühen Entwicklungsstadien. Unter Umständen haben Sie selbst, abhängig von Ihrer Rolle im Entwicklungsprozess, nicht viel mit XAML zu tun. Der Grad, zu dem Sie mit XAML-Dateien interagieren, hängt auch davon ab, welche Entwicklungsumgebung Sie verwenden, ob Sie auf interaktive Designumgebungsfunktionen wie Toolboxen und Eigenschaften-Editoren zurückgreifen, wie groß der Umfang Ihrer Windows-Runtime-App ist und welchem Zweck sie dient. Trotzdem ist es wahrscheinlich, dass Sie während der Entwicklung Ihrer App eine XAML-Datei auf Elementebene mithilfe eines Text- oder XML-Editors bearbeiten werden.

## Mit diesen Informationen können Sie XAML-Dateien in einer Text- oder XML-Darstellung sicher bearbeiten und die Gültigkeit der Deklarationen und den Zweck der Dateien bewahren, wenn sie von Tools, Markupkompilierungsvorgängen oder in der Laufzeitphase Ihrer Windows-Runtime-App verarbeitet werden.

Optimieren des XAML-Codes für hohe Auslastung Im Folgenden sind einige Tipps zum Definieren von UI-Elementen in XAML-Code mithilfe der bewährten Methoden zur Verbesserung der Leistung aufgeführt. Viele dieser Tipps beziehen sich auf die Verwendung von XAML-Ressourcen. Sie wurden zur besseren Verständlichkeit jedoch auch hier in der allgemeinen XAML-Übersicht angegeben. Weitere Informationen zu XAML-Ressourcen finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](https://msdn.microsoft.com/library/windows/apps/mt187273).

-   Weitere Tipps zur Leistung, z. B. XAML-Beispielcode, in dem absichtlich einige Praktiken veranschaulicht werden, die zu schlechter Leistung führen und vermieden werden sollten, finden Sie unter [Optimieren des XAML-Markups](https://msdn.microsoft.com/library/windows/apps/mt204779).
-   Wenn Sie im XAML-Code häufig dieselbe Pinselfarbe verwenden, sollten Sie ein [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962)-Element als Ressource einrichten, anstatt jedes Mal eine benannte Farbe als Attributwert zu nutzen. Falls Sie dieselbe Ressource auf mehr als einer UI-Seite verwenden, empfiehlt sich die Definition unter [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338), anstatt auf jeder Seite. Falls eine Ressource nur von einer Seite genutzt wird, ist die Definition unter **Application.Resources** nicht ratsam. Definieren Sie die Ressource stattdessen nur für die jeweilige Seite.
-   Dies ist sowohl für die XAML-Zerlegung beim Entwerfen der App als auch zur Steigerung der Leistung während der XAML-Analyse hilfreich. Führen Sie für Ressourcen, die von der App verpackt werden, eine Überprüfung auf ungenutzte Ressourcen durch (Ressourcen mit Schlüssel, für die in der App jedoch kein entsprechender [StaticResource](staticresource-markup-extension.md)-Verweis enthalten ist).
-   Entfernen Sie diese vor der Freigabe der App aus dem XAML-Code. Wenn Sie separate XAML-Dateien verwenden, mit denen Designressourcen bereitgestellt werden ([**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801)), können Sie erwägen, ungenutzte Ressourcen in diesen Dateien auszukommentieren oder ganz zu entfernen.
-   Auch wenn Sie über einen gemeinsamen XAML-Startpunkt verfügen, den Sie in mehr als einer App verwenden oder über den häufig verwendete Ressourcen für alle Apps bereitgestellt werden, werden die XAML-Ressourcen doch immer von Ihrer App verpackt und ggf. geladen.
-   Definieren Sie keine UI-Elemente, die Sie für die Komposition nicht benötigen, und verwenden Sie nach Möglichkeit immer die Standardsteuerelementvorlagen (diese Vorlagen wurden bereits auf hohe Leistungsfähigkeit getestet und bestätigt). Nutzen Sie Container wie [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250), anstatt freiwillige Überzeichnungen von UI-Elementen. Einfach ausgedrückt: Zeichnen Sie dasselbe Pixel nicht mehrere Male.
-   Weitere Informationen zu Überzeichnungen und den damit verbundenen Tests finden Sie unter [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/hh701823).

## Verwenden Sie die Standardelementvorlagen für [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) oder [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705); dieser verfügen über eine spezielle **Presenter**-Logik, die Leistungsprobleme beim Erstellen der visuellen Struktur für eine große Anzahl von Listenelementen behebt.

Debuggen von XAML Da es sich bei XAML um eine Markupsprache handelt, sind einige der typischen Strategien für das Debuggen innerhalb von Microsoft Visual Studio nicht verfügbar. Beispielsweise besteht keine Möglichkeit zum Festlegen eines Haltepunkts innerhalb einer XAML-Datei.

Jedoch bestehen andere Techniken, mit denen Sie Probleme mit UI-Definitionen oder XAML-Markups während der Appentwicklung beheben können. Treten Probleme mit einer XAML-Datei auf, wird sehr wahrscheinlich von einem System oder Ihrer App eine XAML-Analyseausnahme ausgelöst. Tritt eine XAML-Analyseausnahme auf, konnte von der vom XAML-Parser geladenen XAML keine gültige Objektstruktur erstellt werden.

In einigen Fällen, z.B. wenn die XAML die erste Seite der Anwendung darstellt, die als die visuelle Stammstruktur geladen wird, ist die XAML-Analyseausnahme nicht behebbar. XAML wird häufig innerhalb einer IDE, z.B. Visual Studio, und einer der XAML-Designoberflächen bearbeitet. Visual Studio kann in vielen Fällen Überprüfung zu Entwurfszeit und Fehlerüberprüfung einer XAML-Quelle während der Bearbeitung bereitstellen.

Beispielsweise werden ungültige Attributwerte im XAML-Texteditor bei der Eingabe unter Umständen durch Wellenlinien gekennzeichnet, und Sie sehen noch vor der XAML-Kompilierung, dass die Benutzeroberflächendefinition fehlerhaft ist. Wird die App ausgeführt und wurden XAML-Analysefehler zur Entwurfszeit nicht erfasst, werden werden sie von der Common Language Runtime (CLR) als [**XamlParseException**](https://msdn.microsoft.com/library/windows/apps/hh673774) gemeldet.

Weitere Infos dazu, was im Falle einer Laufzeit-**XamlParseException** unternommen werden kann, finden Sie unter [Ausnahmebehandlung für Windows-Runtime-Apps in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/dn532194). 
            **Hinweis:** Apps mit C++/CX-Code erhalten nicht das bestimmte [**XamlParseException**](https://msdn.microsoft.com/library/windows/apps/hh673774)-Element.

Die Meldung in der Ausnahme stellt die Fehlerursache als XAML-bezogen dar und enthält Kontextinfos, z.B. Zeilennummern in einer XAML-Datei, so wie **XamlParseException**.




<!--HONumber=Jun16_HO4-->


