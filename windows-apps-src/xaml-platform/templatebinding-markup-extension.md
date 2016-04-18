---
description: Verknüpft den Wert einer Eigenschaft in einer Steuerelementvorlage mit dem Wert einer anderen Eigenschaft, die im Steuerelement mit Vorlagen verfügbar gemacht wird. TemplateBinding kann nur in einer ControlTemplate-Definition in XAML verwendet werden.
title: TemplateBinding-Markuperweiterung
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
---

# {TemplateBinding}-Markuperweiterung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Verknüpft den Wert einer Eigenschaft in einer Steuerelementvorlage mit dem Wert einer anderen Eigenschaft, die im Steuerelement mit Vorlagen verfügbar gemacht wird. **TemplateBinding** kann nur in einer [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)-Definition in XAML verwendet werden.

## XAML-Attributsyntax

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## XAML-Attributsyntax (für die Setter-Eigenschaft in einer Vorlage oder Formatvorlage)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## XAML-Werte

| Benennung | Beschreibung |
|------|-------------|
| propertyName | Der Name der Eigenschaft, die in der Setter-Syntax festgelegt wird. Diese Eigenschaft muss eine Abhängigkeitseigenschaft sein. |
| sourceProperty | Der Name einer anderen Abhängigkeitseigenschaft, die für den als Vorlage festgelegten Typ vorhanden ist. |

## Anmerkungen

Die Verwendung von **TemplateBinding** ist ein grundlegender Punkt beim Definieren einer Steuerelementvorlage, wenn Sie entweder ein Autor von benutzerdefinierten Steuerelementen sind oder eine Steuerelementvorlage für vorhandene Steuerelemente ersetzen. Weitere Informationen finden Sie unter [Schnellstart: Steuerelementvorlagen](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374).

Für *propertyName* und *targetProperty* wird häufig derselbe Eigenschaftsname verwendet. In diesem Fall kann ein Steuerelement eine Eigenschaft für sich selbst definieren und diese an eine vorhandene, intuitiv benannte Eigenschaft eines seiner Komponententeile weiterleiten. Ein Steuerelement, dessen Zusammensetzung einen zum Anzeigen seiner **Text**-Eigenschaft verwendeten [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) beinhaltet, kann z. B. diesen XAML-Code als Teil der Steuerelementvorlage enthalten: `<TextBlock Text="{TemplateBinding Text}" .... />`

Die Typen, die als Wert für die Quelleigenschaft und die Zieleigenschaft verwendet werden, müssen übereinstimmen. Bei Verwendung von **TemplateBinding** gibt es keine Möglichkeit, einen Konverter zu nutzen. Falls die Werte nicht übereinstimmen, tritt beim Analysieren des XAML-Codes ein Fehler auf. Wenn Sie einen Konverter benötigen, können Sie die ausführliche Syntax für eine Vorlagenbindung verwenden, z. B.: `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

Wenn Sie versuchen, eine **TemplateBinding** außerhalb einer [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)-Definition in XAML zu verwenden, führt dies zu einem Parserfehler.

Sie können **TemplateBinding** für Fälle verwenden, in denen der übergeordnete Wert mit Vorlagen auch als weitere Bindung zurückgestellt wird. Die Auswertung für **TemplateBinding** kann warten, bis etwaige erforderliche Laufzeitbindungen über Werte verfügen.

Ein **TemplateBinding**-Element ist stets eine unidirektionale Bindung. Bei beiden beteiligten Eigenschaften muss es sich um Abhängigkeitseigenschaften handeln.

**TemplateBinding** ist eine Markuperweiterung. Markuperweiterungen werden in der Regel implementiert, wenn für Attributwerte Escapezeichen verwendet werden müssen, damit sie keine Literalwerte oder Handlernamen darstellen, und es nicht ausreicht, Typkonverter für bestimmte Typen oder Eigenschaften zu verwenden. Alle Markuperweiterungen in XAML verwenden die Zeichen „{” und „}” in ihrer Attributsyntax. Anhand dieser Konvention erkennt ein XAML-Prozessor, dass eine Markuperweiterung das Attribut verarbeiten muss.

**Hinweis**  Die XAML-Prozessorimplementierung der Windows-Runtime enthält keine Sicherungsklassendarstellung für **TemplateBinding**. **TemplateBinding** ist ausschließlich für die Verwendung im XAML-Markup vorgesehen. Es gibt keine einfache Methode zum Reproduzieren des Verhaltens im Code.

## Verwandte Themen

* [Schnellstart: Steuerelementvorlagen](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)
* [Übersicht über XAML](xaml-overview.md)
* [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md)
 



<!--HONumber=Mar16_HO1-->


