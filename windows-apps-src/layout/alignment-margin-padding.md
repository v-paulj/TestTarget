---
author: Jwmsft
Description: Verwenden Sie Ausrichtung, Rand und Abstand, um das Layout von Elementen auf einer Seite zu steuern.
title: "Ausrichtung, Rand und Abstand für UWP-Apps (Universelle Windows-Plattform)"
ms.assetid: 9412ABD4-3674-4865-B07D-64C7C26E4842
label: Alignment, margin, and padding
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 86635255fbdae83fb2749e2aea7011a8b989e83f

---
# Ausrichtung, Rand und Abstand

Neben Abmessungseigenschaften (Breite, Höhe und Beschränkungen) können Elemente auch Ausrichtungs-, Rand- und Abstandseigenschaften aufweisen, die das Layoutverhalten beeinflussen, wenn ein Element einen Layoutdurchlauf absolviert und auf einer Benutzeroberfläche dargestellt wird. Es bestehen Beziehungen zwischen Ausrichtungs-, Rand-, Abstands- und Abmessungseigenschaften, die beim Positionieren eines [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)-Objekts eine typische Logikabfolge aufweisen. Demzufolge werden Objekte den Umständen entsprechend manchmal verwendet und manchmal ignoriert.

## Ausrichtungseigenschaften

Die [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720)-Eigenschaft und die [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)-Eigenschaft beschreiben, wie ein untergeordnetes Element innerhalb des zugeordneten Layoutbereichs eines übergeordneten Elements positioniert werden sollte. Durch die Kombination dieser Eigenschaften kann die Layoutlogik für einen Container untergeordnete Elemente innerhalb des Containers positionieren (entweder ein Bereich oder ein Steuerelement). Ausrichtungseigenschaften sollen das gewünschte Layout für einen adaptiven Layoutcontainer andeuten. Demzufolge werden sie für untergeordnete [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)-Elemente festgelegt und von einem anderen übergeordneten **FrameworkElement**-Container interpretiert. Ausrichtungswerte können angeben, ob die Elemente an einem der beiden Ränder einer Ausrichtung oder an der Mitte ausgerichtet werden. Der Standardwert für beide Ausrichtungseigenschaften lautet jedoch **Stretch**. Bei der **Stretch**-Ausrichtung füllen die Elemente den Bereich, der im Layout bereitgestellt wird. **Stretch** ist die Standardeinstellung. Damit ist es einfacher, in den Fällen, in denen keine explizite Messung erfolgt oder kein [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)-Wert aus dem Messungsdurchlauf des Layouts vorliegt, adaptive Layouttechniken zu verwenden. Mit dieser Standardeinstellung wird das Risiko vermieden, dass eine explizite Höhe/Breite nicht in den Container passt und vor der Größenfestlegung des Containers beschnitten wird.

> **Hinweis**
            &nbsp;&nbsp;Als generelles Layoutprinzip gilt: Wenden Sie Messungen nur auf bestimmte Schlüsselelemente an und nutzen Sie für die anderen Elemente das adaptive Layoutverhalten. Dadurch wird ein flexibles Layoutverhalten für den Fall bereitgestellt, dass der Benutzer die Größe des oberen App-Fensters festlegt. Dies ist normalerweise jederzeit möglich.

 
Wenn in einem adaptiven Container [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718)- und [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751)-Werte vorhanden sind oder eine Beschneidung erfolgt, wird das Layout auch dann durch das Verhalten des zugehörigen Containers gesteuert, wenn **Stretch** als Ausrichtungswert festgelegt ist. In Bereichen agiert ein **Stretch**-Wert, der durch **Height** und **Width** ausgeschlossen wurde, als würde der Wert **Center** lauten.

Wenn keine natürlichen oder berechneten Höhen- und Breitenwerte vorhanden sind, sind diese Abmessungswerte mathematisch **NaN** (keine Zahl). Die Elemente warten auf die Zuweisung von Abmessungen durch den zugehörigen Layoutcontainer. Nach Ausführung des Layouts liegen für die Elemente, für die eine **Stretch**-Ausrichtung verwendet wurde, Werte für die [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707)- und [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)-Eigenschaften vor. Die **NaN**-Werte bleiben in [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) und [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) für die untergeordneten Elemente erhalten, sodass das adaptive Verhalten erneut ausgeführt werden kann. Dies kann beispielsweise der Fall sein, wenn layoutbezogene Änderungen, wie die Größenänderung des App-Fensters, einen weiteren Layoutzyklus verursachen.

Für Textelemente wie [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) ist normalerweise keine explizit deklarierte Breite vorhanden. Sie haben jedoch eine berechnete Breite, die Sie mithilfe von [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) abfragen können. Diese Breite hebt zudem eine **Stretch**-Ausrichtung auf. (Die [**FontSize**](https://msdn.microsoft.com/library/windows/apps/br209657)-Eigenschaft und weitere Texteigenschaften sowie der Text selbst deuten bereits auf die beabsichtigte Layoutgröße hin. Text sollte normalerweise nicht gestreckt werden.) Innerhalb eines Steuerelements als Inhalt verwendeter Text hat den gleichen Effekt. Durch das Vorhandensein von Text, der dargestellt werden muss, wird eine **ActualWidth**-Berechnung verursacht. Dadurch wird zudem eine gewünschte Breite und Größe an das enthaltende Steuerelement angepasst. Textelemente verfügen auch über eine [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707), die auf der Schriftgröße pro Zeile, Zeilenumbrüchen und anderen Texteigenschaften basiert.

Ein Bereich wie [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) verfügt bereits über eine andere Layoutlogik (Zeilen- und Spaltendefinitionen und angehängte Eigenschaften wie [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795), die für Elemente festgelegt werden, um anzugeben, in welcher Zelle gezeichnet werden soll). In diesem Fall beeinflussen die Ausrichtungseigenschaften die Art und Weise, wie der Inhalt innerhalb des Bereichs dieser Zelle ausgerichtet wird. Die Zellenstruktur und Größenfestlegung wird jedoch durch **Grid**-Einstellungen gesteuert.

Steuerelemente für Elemente zeigen mitunter Elemente an, deren Basistypen Daten sind. Dabei ist ein [**ItemsPresenter**](https://msdn.microsoft.com/library/windows/apps/br242843) beteiligt. Obwohl die Daten selbst kein vom [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) abgeleiteter Typ sind, ist dies für den **ItemsPresenter** der Fall. Demzufolge können Sie [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) und [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749) für den Präsentierer festlegen, und diese Ausrichtung gilt bei der Darstellung in der Elementsteuerung für die Datenelemente.

Die Ausrichtungseigenschaften sind nur in den Fällen relevant, in denen in einer Dimension des übergeordneten Layoutcontainers Freiraum verfügbar ist. Wenn in einem Layoutcontainer Inhalte bereits beschnitten werden, kann sich die Ausrichtung auf den Bereich des Elements auswirken, auf den die Beschneidung angewendet wird. Wenn Sie beispielsweise `HorizontalAlignment="Left"` festlegen, wird die korrekte Größe des Elements beschnitten.

## Rand

Die [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724)-Eigenschaft beschreibt den Abstand zwischen einem Element und seinen Peers in einer Layoutsituation sowie den Abstand zwischen einem Element und dem Inhaltsbereich eines Containers, der das Element enthält. Betrachtet man Elemente als Begrenzungsrahmen oder Rechtecke, in denen [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) und [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) die Abmessungen darstellen, wird das **Margin**-Layout auf die Außenseite dieses Rechtecks angewendet, und **ActualHeight** und **ActualWidth** werden keine Pixel hinzugefügt. Der Rand wird auch nicht als Teil des Elements betrachtet, das für Treffertests und Eingabeereignisse der Quellenentnahme verwendet wird.

In einem allgemeinen Layoutverhalten werden die Komponenten eines [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724)-Werts zuletzt beschränkt, und sie werden zudem erst beschränkt, nachdem [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) und [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) vollständig auf 0 beschränkt wurden. Gehen Sie daher mit Rändern vorsichtig vor, wenn der Container das Element bereits beschneidet oder beschränkt. Andernfalls kann ein Rand die Ursache dafür sein, dass ein Element scheinbar nicht dargestellt wird (weil eine seiner Abmessungen nach der Anwendung des Rands auf 0 beschränkt wurde).

Mit einer Syntax wie `Margin="20"` können Sie einheitliche Randwerte festlegen. Mit dieser Syntax wird ein einheitlicher Rand von 20 Pixeln auf das Element angewendet, und zwar ein 20-Pixel-Rand an der linken, oberen, rechten und unteren Seite. Randwerte können auch in Form unterschiedlicher Werte auftreten, wobei jeder Wert für einen unterschiedlichen Rand steht, der auf die linke, obere, rechte und untere Seite angewendet wird (in dieser Reihenfolge). Beispiel: `Margin="0,10,5,25"`. Der zugrunde liegende Typ für die [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724)-Eigenschaft ist eine [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864)-Struktur mit Eigenschaften, die [**Left**](https://msdn.microsoft.com/library/windows/apps/hh673893)-, [**Top**](https://msdn.microsoft.com/library/windows/apps/hh673840)-, [**Right**](https://msdn.microsoft.com/library/windows/apps/hh673881)- und [**Bottom**](https://msdn.microsoft.com/library/windows/apps/hh673775)-Werte als separate **Double**-Werte enthalten.

Ränder sind additiv. Wenn beispielsweise zwei Elemente jeweils einen einheitlichen Rand von 10 Pixeln angeben und es sich dabei um benachbarte Peers mit beliebiger Ausrichtung handelt, beträgt der Abstand zwischen den Elementen 20 Pixel.

Negative Ränder sind zulässig. Die Verwendung eines negativen Rands kann jedoch oftmals Beschneidungen oder Überzeichnungen von Peers verursachen. Die Verwendung negativer Ränder ist daher keine übliche Technik.

Die ordnungsgemäße Verwendung der [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724)-Eigenschaft ermöglicht eine sehr präzise Steuerung der Darstellungsposition eines Elements und der Darstellungsposition der benachbarten und untergeordneten Elemente. Wenn Sie Elemente ziehen, um sie im XAML-Designer in Visual Studio zu positionieren, werden Sie feststellen, dass die geänderte XAML-Datei **Margin**-Werte dieses Elements aufweist, mit deren Hilfe die Positionierungsänderungen wieder in der XAML-Datei serialisiert wurden.

Die [**Block**](https://msdn.microsoft.com/library/windows/apps/br244379)-Klasse, bei der es sich um eine Basisklasse für [**Paragraph**](https://msdn.microsoft.com/library/windows/apps/br244503) handelt, verfügt ebenfalls über eine [**Margin**](https://msdn.microsoft.com/library/windows/apps/jj191725)-Eigenschaft. Sie hat eine analoge Auswirkung darauf, wie dieser **Paragraph** in seinem übergeordneten Container positioniert wird (dabei handelt es sich normalerweise um ein [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/br227565)- oder [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548)-Objekt) und auch darauf, wie mehrere Absätze im Verhältnis zu anderen **Block**-Peers der [**RichTextBlock.Blocks**](https://msdn.microsoft.com/library/windows/apps/br244347)-Sammlung positioniert werden.

## Abstand

Eine **Padding**-Eigenschaft beschreibt den Abstand zwischen einem Element und beliebigen untergeordneten Elementen oder Inhalten, die es enthält. Inhalte werden als einzelner Begrenzungsrahmen behandelt, der alle Inhalte einschließt, sofern es sich um ein Element handelt, für das mehrere untergeordnete Elemente zulässig sind. Wenn es sich beispielsweise um ein [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803)-Element handelt, das zwei Elemente enthält, wird die [**Padding**](https://msdn.microsoft.com/library/windows/apps/br209459)-Eigenschaft um den Begrenzungsrahmen herum angewendet, in dem die Elemente enthalten sind. Der **Padding**-Wert wird bei der Mess- und Anordnungsdurchlaufsberechnung von der verfügbaren Größe subtrahiert und ist Teil der gewünschten Größenwerte, wenn der Container selbst dem Layoutdurchlauf für die Inhalte unterzogen wird. Im Gegensatz zu [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) ist **Padding** keine [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)-Eigenschaft. Vielmehr sind verschiedene Klassen vorhanden, die jeweils ihre eigene **Padding**-Eigenschaft definieren:

-   [
              **Control.Padding**
            ](https://msdn.microsoft.com/library/windows/apps/br209459): Wird an alle von [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) abgeleiteten Klassen vererbt. Nicht alle Steuerelemente weisen Inhalte auf, sodass die Festlegung der Eigenschaft bei einigen Steuerelementen (z. B. [**AppBarSeparator**](https://msdn.microsoft.com/library/windows/apps/dn279268)) keinerlei Auswirkung hat. Wenn das Steuerelement einen Rahmen aufweist (siehe [**Control.BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209399)), wird der Abstand innerhalb dieses Rahmens angewendet.
-   [
              **Border.Padding**
            ](https://msdn.microsoft.com/library/windows/apps/br209263): Definiert den Abstand zwischen der Rechteckslinie, die von [**BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209256)/[**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209254) erstellt wird, und dem [**Child**](https://msdn.microsoft.com/library/windows/apps/br209258)-Element.
-   [
              **ItemsPresenter.Padding**
            ](https://msdn.microsoft.com/library/windows/apps/hh968021): Trägt zur Darstellung der generierten visuellen Objekte für Elemente in Elementsteuerelementen bei. Dabei wird der angegebene Abstand um die einzelnen Elemente herum platziert.
-   [
              **TextBlock.Padding**
            ](https://msdn.microsoft.com/library/windows/apps/br209673) und [**RichTextBlock.Padding**](https://msdn.microsoft.com/library/windows/apps/br227596): Erweitern den Begrenzungsrahmen um den Text des Textelements. Diese Textelemente haben keinen **Background**. Demzufolge kann der Textabstand im Vergleich zu anderen Layoutverhalten, die durch den Container des Textelements angewendet werden, schwer erkennbar sein. Aus diesem Grund wird der Textelementabstand nur selten verwendet, und häufiger kommen [**Margin**](https://msdn.microsoft.com/library/windows/apps/jj191725)-Einstellungen für enthaltene [**Block**](https://msdn.microsoft.com/library/windows/apps/br244379)-Container zum Einsatz (für den [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/br227565)-Fall).

In all diesen Fällen hat ein und dasselbe Element auch eine **Margin**-Eigenschaft. Wenn sowohl ein Rand als auch ein Abstand angewendet wird, sind sie dahingehend additiv, dass der offensichtliche Abstand zwischen einem äußeren Container und beliebigen inneren Containern dem Rand plus dem Abstand entspricht. Wenn auf Inhalte, Elemente oder Container unterschiedliche Hintergrundwerte angewendet werden, ist der Punkt, an dem der Rand endet und der Abstand beginnt, in der Darstellung potenziell sichtbar.

## Dimensionen (Höhe, Breite)

Die [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718)-Eigenschaft und die [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751)-Eigenschaft für ein [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) haben oftmals Einfluss darauf, wie sich die Ausrichtungs-, Rand- und Abstandseigenschaften bei einem Layoutdurchlauf verhalten. Ein **Height**- und **Width**-Wert in Form einer reellen Zahl bricht insbesondere **Stretch**-Ausrichtungen ab und wird als mögliche Komponente des [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)-Werts heraufgestuft, der während des Messdurchlaufs des Layouts bestimmt wird. **Height** und **Width** weisen Beschränkungseigenschaften auf: Der **Height**-Wert kann durch [**MinHeight**](https://msdn.microsoft.com/library/windows/apps/br208731) und [**MaxHeight**](https://msdn.microsoft.com/library/windows/apps/br208726) und der **Width**-Wert durch [**MinWidth**](https://msdn.microsoft.com/library/windows/apps/br208733) und [**MaxWidth**](https://msdn.microsoft.com/library/windows/apps/br208728) beschränkt werden. Zudem werden [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) und [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) berechnet. Dabei handelt es sich um schreibgeschützte Eigenschaften, die nur nach Abschluss eines Layoutdurchlaufs gültige Werte enthalten. Weitere Informationen dazu, wie die Abmessungen und Beschränkungen oder berechnete Eigenschaften miteinander in Verbindung stehen, finden Sie in den „Anmerkungen“ zu [**FrameworkElement.Height**](https://msdn.microsoft.com/library/windows/apps/br208718) und [**FrameworkElement.Width**](https://msdn.microsoft.com/library/windows/apps/br208751).

## Verwandte Themen

**Referenzen**

[**FrameworkElement.Height**](https://msdn.microsoft.com/library/windows/apps/br208718)

[**FrameworkElement.Width**](https://msdn.microsoft.com/library/windows/apps/br208751)

[**FrameworkElement.HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720)

[**FrameworkElement.VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)

[**FrameworkElement.Margin**](https://msdn.microsoft.com/library/windows/apps/br208724)

[**Control.Padding**](https://msdn.microsoft.com/library/windows/apps/br209459)



<!--HONumber=Jun16_HO4-->


