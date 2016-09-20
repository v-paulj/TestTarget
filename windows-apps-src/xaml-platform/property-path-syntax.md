---
author: jwmsft
description: "Sie können die PropertyPath-Klasse und die Zeichenfolgensyntax verwenden, um einen PropertyPath-Wert entweder in XAML oder in Code zu instanziieren."
title: PropertyPath-Syntax
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 0b1851bc9d19de5b678f8c6c3a255c0ba3057a85

---

# PropertyPath-Syntax

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Sie können die [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)-Klasse und die Zeichenfolgensyntax verwenden, um einen **PropertyPath**-Wert entweder in XAML oder in Code zu instanziieren. 
            **PropertyPath**-Werte werden von Datenbindungen genutzt. Eine ähnliche Syntax kommt für die Ausrichtung von Storyboardanimationen zum Einsatz. Die Animationsausrichtung generiert jedoch keine zugrunde liegenden Property-path-Syntaxwerte. Sie behält die Informationen als Zeichenfolge bei. In beiden Szenarien beschreibt ein Eigenschaftspfad eine Traversierung von einer oder mehreren Objekt-Eigenschaft-Beziehungen, die schließlich in einer einzelnen Eigenschaft aufgelöst werden.

Sie können eine Eigenschaftspfad-Zeichenfolge in XAML direkt einem Attribut zuweisen. Sie haben die Möglichkeit, mit derselben Zeichenfolgensyntax entweder eine [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)-Klasse zu konstruieren, die ein [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Objekt im Code festlegt, oder mithilfe der [**SetTargetProperty**](https://msdn.microsoft.com/library/windows/apps/br210503)-Methode ein Animationsziel im Code zu bestimmen. Es bestehen zwei unterschiedliche Featurebereiche in der Windows-Runtime, die auf einen Eigenschaftspfad zurückgreifen: Datenbindung und Animationsausrichtung. Die Animationsausrichtung generiert keine zugrunde liegenden Property-path-Syntaxwerte in der Windows-Runtime-Implementierung. Sie behält die Informationen als Zeichenfolge bei. Die Konzepte der Objekt-Eigenschaft-Traversierung sind jedoch sehr ähnlich. Datenbindung und Animationsausrichtung werten einen Eigenschaftspfad auf leicht unterschiedliche Weise aus, daher behandeln wir die Eigenschaftspfadsyntax dafür getrennt.

## Eigenschaftspfad für Objekte bei der Datenbindung

In der Windows-Runtime können Sie eine Bindung zum Zielwert einer beliebigen Abhängigkeitseigenschaft vornehmen. Der Quelleigenschaftswert für eine Datenbindung muss keine Abhängigkeitseigenschaft sein. Sie kann eine Eigenschaft auf einem Geschäftsobjekt sein (zum Beispiel eine in einer Microsoft .NET-Sprache oder in C++ geschriebene Klasse ). Das Quellobjekt für den Bindungswert kann auch ein bestehendes Abhängigkeitsobjekt sein, das von der App bereits definiert ist. Die Quelle kann entweder von einem einfachen Eigenschaftennamen oder einer Traversierung der Objekt-Eigenschaft-Beziehungen im Objektgraphen des Geschäftsobjekts referenziert werden.

Sie können eine Bindung zu einem einzelnen Eigenschaftswert oder zu einer solchen Zieleigenschaft vornehmen, die Listen oder Sammlungen enthält. Wenn es sich bei der Quelle um eine Sammlung handelt oder der Pfad eine Sammlungseigenschaft angibt, gleicht das Datenbindungsmodul die Sammelelemente der Quelle mit dem Bindungsziel ab. Dies kann zum Beispiel bewirken, dass ein [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868)-Objekt mit einer Liste von Elementen von einer Datenquellensammlung gefüllt wird, ohne dass die spezifischen Elemente in der Sammlung erwartet werden müssen.

### Traversieren eines Objektgraphen

Das Element der Syntax, das die Traversierung einer Objekt-Eigenschaft-Beziehung in einem Objektgraphen kennzeichnet, ist das Punktzeichen (**.**). Jeder Punkt in einer Eigenschaftspfad-Zeichenfolge gibt eine Trennung zwischen einem Objekt (auf der linken Seite des Punkts) und einer Eigenschaft dieses Objekts (auf der rechten Seite des Punkts) an. Die Zeichenfolge wird von links nach rechts ausgewertet, was ermöglicht, mehrere Objekt-Eigenschaft-Beziehungen zu durchlaufen. Sehen wir uns dazu ein Beispiel an:

``` syntax
<Binding Path="Customer.Address.StreetAddress1"
```

Dieser Pfad wird wie folgt ausgewertet:

1.  Das Datenkontextobjekt (oder eine mit derselben [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Klasse angegebene [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832)-Eigenschaft) wird nach einer Eigenschaft namens „Customer“ durchsucht.
2.  Das Objekt, das den Wert der „Customer“-Eigenschaft darstellt, wird nach einer Eigenschaft namens „Address“ durchsucht.
3.  Das Objekt, das den Wert der „Customer“-Eigenschaft darstellt, wird nach einer Eigenschaft namens „StreetAddress1“ durchsucht.

Bei jedem dieser Schritte wird der Wert als Objekt behandelt. Der Typ des Ergebnisses wird nur dann überprüft, wenn die Bindung auf eine bestimmte Eigenschaft angewendet wird. Dieses Beispiel würde nicht funktionieren, wenn es sich bei „Address“ nur um einen Zeichenfolgenwert handeln würde, der nicht klarstellt, bei welchem Teil der Zeichenfolge es sich um die Straßenadresse handelt. In der Regel verweist die Bindung auf bestimmte geschachtelte Eigenschaftswerte eines Geschäftsobjekts, das eine bekannte und zweckmäßige Informationsstruktur aufweist.

### Regeln für die Eigenschaften in einem Datenbindungs-Eigenschaftspfad

-   Alle Eigenschaften, die von einem Eigenschaftspfad referenziert werden, müssen im Quellgeschäftsobjekt öffentlich sein.
-   Die Endeigenschaft (die Eigenschaft, bei der es sich um die letzte ausgewiesene Eigenschaft im Pfad handelt) muss öffentlich und änderbar sein. Es ist nicht möglich, Bindungen zu statischen Werten herzustellen.
-   Die Endeigenschaft muss „read/write“ sein, wenn dieser Pfad als [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)-Information in einer bidirektionalen Bindung verwendet werden soll.

### Indexer

Ein Eigenschaftspfad für Datenbindungen kann Verweise auf indizierte Eigenschaften aufweisen. Das ermöglicht die Bindung an geordnete Listen/Vektoren oder Wörterbücher/Karten. Verwenden Sie eckige Klammern „\[\]“, um eine indizierte Eigenschaft anzugeben. Diese Klammern können entweder eine ganze Zahl (für eine geordnete Liste) oder eine Zeichenfolge ohne Anführungszeichen (für Wörterbücher) enthalten. Darüber hinaus ist die Bindung an ein Wörterbuch möglich, bei der der Schlüssel eine ganze Zahl ist. Sie können verschiedene indizierte Eigenschaften im selben Pfad verwenden und die Objekteigenschaft mit einem Punkt abtrennen.

Nehmen wir zum Beispiel ein Geschäftsobjekt, bei dem es eine Liste von „Teams“ gibt (geordnete Liste), von denen jedes ein Wörterbuch von „Players“ aufweist, wobei als Schlüssel für jeden Spieler der Nachname verwendet wird. Ein Beispiel eines Eigenschaftspfads zu einem bestimmen Spieler im zweiten Team lautet: „Teams\[1\].Players\[Smith\]“. (Sie verwenden 1, um das zweite Element in „Teams“ anzugeben, da die Liste nullindiziert ist.)


            **Hinweis**  Die Unterstützung der Indizierung für C++-Datenquellen ist beschränkt. Informationen hierzu finden Sie unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

### Angefügte Eigenschaften

Eigenschaftspfade können Verweise auf angefügte Eigenschaften enthalten. Da der Bezeichnername einer angefügten Eigenschaft bereits einen Punkt enthält, muss ein Name für die angefügte Eigenschaft in runden Klammern hinzugefügt werden, damit der Punkt nicht als Objekteigenschaftsschritt interpretiert wird. Wenn Sie beispielsweise [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773) als Bindungspfad verwenden möchten, lautet die korrekte Zeichenfolge „(Canvas.ZIndex)“. Weitere Informationen zu angefügten Eigenschaften finden Sie unter [Übersicht über angefügte Eigenschaften](attached-properties-overview.md).

### Zusammenführen der Eigenschaftspfadsyntax

Sie können verschiedene Elemente der Eigenschaftspfadsyntax in einer einzelnen Zeichenfolge zusammenführen. Beispielsweise können Sie einen Eigenschaftspfad definieren, der eine indizierte angefügte Eigenschaft referenziert, sofern Ihre Datenquelle eine solche Eigenschaft aufweist.

### Debuggen eines Bindungseigenschaftspfads

Da ein Eigenschaftspfad von einem Bindungsmodul interpretiert wird und auf Informationen basiert, die möglicherweise nur während der Laufzeit verfügbar sind, müssen Sie einen Eigenschaftspfad für Bindungen oft debuggen, ohne dass Ihnen die gewöhnliche Entwurfszeit- oder Kompilierungszeitunterstützung in den Entwicklungstools zur Verfügung steht. In vielen Fällen ist das Laufzeitergebnis bei einer fehlgeschlagenen Auflösung eines Eigenschaftspfads ein leerer Wert ohne Fehler, da es sich dabei um das Standardausweichverhalten bei der Auflösung von Bindungen handelt. Glücklicherweise bietet Microsoft Visual Studio einen Debugausgabemodus, der den Teil eines Eigenschaftspfads isoliert, der eine Bindungsquelle angibt, die nicht aufgelöst werden konnte. Weitere Informationen zur Verwendung dieses Entwicklungstoolfeatures finden Sie im Abschnitt [„Debuggen“ im Thema „Datenbindung im Detail“](../data-binding/data-binding-in-depth.md#debugging).

## Eigenschaftspfad für die Animationsausrichtung

Animationen stützen sich auf die Ausrichtung auf eine Abhängigkeitseigenschaft, wobei Storyboardwerte angewendet werden, wenn die Animation läuft. Die Animation ermittelt ein Element anhand des Namens ([x:Name-Attribut](x-name-attribute.md)), um das Objekt mit der zu animierenden Eigenschaft zu identifizieren. Oft ist es erforderlich, einen Eigenschaftspfad zu definieren, der mit dem Objekt beginnt, das als der [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823) bezeichnet wird, und mit dem speziellen Abhängigkeitseigenschaftswert endet, bei dem die Animation angewendet werden soll. Dieser Eigenschaftspfad wird als Wert für [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/hh759824) verwendet.

Weitere Informationen zum Definieren von Animationen in XAML finden Sie unter [Storyboardanimationen](https://msdn.microsoft.com/library/windows/apps/mt187354).

## Einfache Ausrichtung

Wenn Sie eine Eigenschaft animieren, die auf dem Zielobjekt selbst existiert, und es möglich ist, eine Animation direkt auf den Typ dieser Eigenschaft anzuwenden (statt auf eine Untereigenschaft des Werts einer Eigenschaft), können Sie die zu animierende Eigenschaft einfach benennen. Es müssen keine weiteren Bedingungen erfüllt sein. Wenn Sie zum Beispiel eine Ausrichtung auf eine [**Shape**](https://msdn.microsoft.com/library/windows/apps/br243377)-Unterklasse wie [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) vornehmen und eine animierte [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723)-Struktur auf die [**Fill**](https://msdn.microsoft.com/library/windows/apps/br243378)-Eigenschaft anwenden, kann der Eigenschaftspfad „Fill“ sein.

## Indirekte Eigenschaftsausrichtung

Sie können eine Eigenschaft animieren, bei der es sich um eine Untereigenschaft des Zielobjekts handelt. Anders ausgedrückt: Wenn das Zielobjekt über eine Eigenschaft verfügt, bei der es sich ebenfalls um ein Objekt handelt, und dieses Objekt Eigenschaften aufweist, müssen Sie einen Eigenschaftspfad definieren, der klarstellt, wie diese Objekt-Eigenschaft-Beziehung durchlaufen werden soll. Wenn Sie ein Objekt angeben, bei dem Sie eine Untereigenschaft animieren möchten, fügen Sie den Eigenschaftennamen in runden Klammern an. Die Eigenschaft geben Sie im Format *Typname*.*Eigenschaftsname* ein. Um beispielsweise anzugeben, dass Sie den Objektwert einer [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980)-Eigenschaft eines Zielobjekts abrufen möchten, legen Sie „(UIElement.RenderTransform)“ als ersten Schritt im Eigenschaftspfad fest. Dabei handelt es sich noch nicht um einen vollständigen Pfad, da es keine Animationen gibt, die sich direkt auf einen [**Transform**](https://msdn.microsoft.com/library/windows/apps/br243006)-Wert anwenden lassen. In diesem Beispiel müssen Sie den Eigenschaftspfad nun vervollständigen. Bei der Endeigenschaft handelt es sich somit um eine Eigenschaft einer **Transform**-Unterklasse, die von einem **Double**-Wert animiert werden kann: „(UIElement.RenderTransform).(CompositeTransform.TranslateX)“

## Angeben eines bestimmten untergeordneten Elements in einer Sammlung

Sie können einen numerischen Indexer dazu verwenden, ein untergeordnetes Element in einer Sammlungseigenschaft anzugeben. Verwenden Sie für den Ganzzahl-Indexwert eckige Klammern „\[\]“. Sie können nur geordnete Listen aber keine Wörterbücher referenzieren. Da es sich bei einer Sammlung nicht um einen Wert handelt, der animiert werden kann, kann eine Indexerverwendung nie die Endeigenschaft in einem Eigenschaftspfad sein.

Mithilfe des folgenden Eigenschaftspfads können Sie beispielsweise angeben, dass die erste Farbe oder Stoppfarbe in einem [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/br210108)-Objekt, das auf die [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395)-Eigenschaft eines Steuerelements angewendet wird, animiert werden soll: „(Control.Background).(GradientBrush.GradientStops)\[0\].(GradientStop.Color)“. Achten Sie darauf, dass der Indexer nicht der letzte Schritt im Pfad ist und dass vor allem der letzte Schritt die [**GradientStop.Color**](https://msdn.microsoft.com/library/windows/apps/br210094)-Eigenschaft des Elements 0 in der Sammlung referenzieren muss, um einen animierten [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723)-Wert darauf anzuwenden.

## Animieren einer angefügten Eigenschaft

Wenn auch dies selten vorkommt, kann eine angefügte Eigenschaft animiert werden, sofern die angefügte Eigenschaft einen Eigenschaftswert aufweist, der mit einem Animationstyp übereinstimmt. Da der Bezeichnername einer angefügten Eigenschaft bereits einen Punkt enthält, muss ein Name für die angefügte Eigenschaft in runden Klammern hinzugefügt werden, damit der Punkt nicht als Objekteigenschaftsschritt interpretiert wird. Verwenden Sie beispielsweise für die Zeichenfolge, die zum Animieren der angefügten [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795)-Eigenschaft auf einem Objekt angegeben werden muss, den Eigenschaftspfad „(Grid.Row)“.


            **Hinweis**  In diesem Beispiel ist der Wert von [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) ein **Int32**-Eigenschaftstyp. Daher ist die Animation mit einer **Double**-Animation nicht möglich. Definieren Sie stattdessen eine [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320)-Klasse, die über [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132)-Komponenten verfügt, wobei [**ObjectKeyFrame.Value**](https://msdn.microsoft.com/library/windows/apps/br210344) auf eine ganze Zahl wie „0“ oder „1“ festgelegt wird.

## Regeln für die Eigenschaften in einem Animationsausrichtungs-Eigenschaftspfad

-   Der angenommene Anfangspunkt des Eigenschaftspfads ist das Objekt, das von einem [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823) bezeichnet wird.
-   Alle über den Eigenschaftspfad hinweg referenzierten Objekte und Eigenschaften müssen öffentlich sein.
-   Die Endeigenschaft (die Eigenschaft, bei der es sich um die letzte ausgewiesene Eigenschaft im Pfad handelt) muss öffentlich, les- und schreibbar und eine Abhängigkeitseigenschaft sein.
-   Die Endeigenschaft muss zudem über einen Eigenschaftstyp verfügen, der von einer der breitgefächerten Klassen von Animationstypen animiert werden kann ([**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723)-Animationen, **Double**-Animationen, [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)-Animationen, [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320)).

## Die PropertyPath-Klasse

Die [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)-Klasse ist der zugrundeliegende Eigenschaftstyp der [**Binding.Path**](https://msdn.microsoft.com/library/windows/apps/br209830)-Eigenschaft für das Bindungsszenario.

Meistens ist es in XAML möglich, eine [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)-Klasse ohne jeglichen Code anzuwenden. In einigen Fällen ist es jedoch sinnvoll, ein **PropertyPath**-Objekt mithilfe von Code zu definieren und dieses zur Laufzeit einer Eigenschaft zuzuordnen.


            [
              **PropertyPath**
            ](https://msdn.microsoft.com/library/windows/apps/br244259) verfügt über einen [**PropertyPath(String)**](https://msdn.microsoft.com/library/windows/apps/br244261)-Konstruktor und hat keinen Standardkonstruktor. Die Zeichenfolge, die Sie diesem Konstruktor übergeben, wird mithilfe der zuvor beschriebenen Eigenschaftspfadsyntax definiert. Dies ist dieselbe Zeichenfolge, mit der Sie [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) als XAML-Attribut zuweisen können. Die einzige andere API der **PropertyPath**-Klasse ist die schreibgeschützte [**Path**](https://msdn.microsoft.com/library/windows/apps/br244260)-Eigenschaft. Sie können diese Eigenschaft als Konstruktionszeichenfolge für eine andere **PropertyPath**-Instanz verwenden.

## Verwandte Themen

* [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [Storyboardanimationen](https://msdn.microsoft.com/library/windows/apps/mt187354)
* [{Binding}-Markuperweiterung](binding-markup-extension.md)
* [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)
* [**Bindung**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**Binding-Konstruktor**](https://msdn.microsoft.com/library/windows/apps/br209825)
* [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)




<!--HONumber=Jun16_HO4-->


