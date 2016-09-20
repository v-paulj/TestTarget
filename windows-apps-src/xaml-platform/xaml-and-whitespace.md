---
author: jwmsft
description: "Informationen zu von XAML verwendeten Regeln für die Leerzeichenverarbeitung."
title: XAML und Leerzeichen
ms.assetid: 025F4A8E-9479-4668-8AFD-E20E7262DC24
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 408c9c7f79f5db81bdf7810a6c71cf25c1c8ec51

---

# XAML und Leerzeichen

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Informationen zu von XAML verwendeten Regeln für die Leerzeichenverarbeitung.

## Leerzeichenverarbeitung

In XAML werden genau wie in XML Leerstellen, Zeilenvorschub und Tabulator als Leerzeichen verwendet. Diese entsprechen den Unicode-Werten "0020", "000A" bzw. "0009". Standardmäßig wird diese Leerzeichennormalisierung angewendet, wenn ein XAML-Verarbeiter auf internen Text zwischen Elementen in einer XAML-Datei stößt:

-   Zeilenvorschubzeichen zwischen ostasiatischen Zeichen werden entfernt.
-   Alle Leerzeichen (Leerstelle, Zeilenvorschub, Tabulator) werden in Leerstellen umgewandelt.
-   Alle aufeinanderfolgenden Leerstellen werden gelöscht und durch eine einzelne Leerstelle ersetzt.
-   Direkt auf das Starttag folgende Leerstellen werden gelöscht.
-   Leerstellen, die sich direkt vor dem Endtag befinden, werden gelöscht.
-   
            Mit *ostasiatischen Zeichen* sind die Unicode-Zeichenbereiche „U+20000 bis U+2FFFD“ und „U+30000 bis U+3FFFD“ gemeint. Diese Teilmenge wird gelegentlich auch als *CJK-Ideogramm* bezeichnet. Weitere Informationen finden Sie unter http://www.unicode.org.

"Default" entspricht dem Zustand, der durch den Standardwert des **xml:space**-Attributs angegeben wird.

### Leerzeichen in internem Text und Zeichenfolgen-Grundtypen

Die oben angegebenen Normalisierungsregeln gelten für internen Text innerhalb von XAML-Elementen. Nach der Normalisierung konvertiert ein XAML-Prozessor sämtlichen internen Text gemäß den folgenden Regeln in einen passenden Typ:

-   Wenn es sich beim Typ der Eigenschaft nicht um eine Sammlung, aber auch nicht direkt um einen **Object**-Typ handelt, versucht der XAML-Prozessor, mithilfe des eigenen Typkonverters eine Konvertierung in diesen Typ auszuführen. Ist diese Konvertierung nicht erfolgreich, tritt ein XAML-Analysefehler auf.
-   Wenn es sich beim Typ der Eigenschaft um eine Sammlung handelt und der interne Text zusammenhängend ist (also keine zusätzlichen Elementtags enthält), wird der interne Text als einzelner **String** analysiert. Wenn der Sammlungstyp keinen **String** akzeptiert, tritt ebenfalls ein XAML-Analysefehler auf.
-   Wenn es sich um eine Eigenschaft vom Typ **Object** handelt, wird der interne Text als einzelner **String** analysiert. Sind zusätzliche Elementtags enthalten, tritt ein XAML-Analysefehler auf, da der Typ **Object** für ein einzelnes Objekt steht (**String** oder ein anderes Objekt).
-   Wenn es sich beim Typ der Eigenschaft um eine Sammlung handelt und der interne Text nicht zusammenhängend ist, wird die erste Teilzeichenfolge in einen **String** konvertiert und als Sammlungselement hinzugefügt. Das Zwischenelement wird ebenfalls als Sammlungselement hinzugefügt, und schließlich wird der Sammlung die nachgestellte Teilzeichenfolge (sofern vorhanden) als drittes **String**-Element hinzugefügt.

### Leerzeichen und Textinhaltsmodelle

In der Praxis ist der Erhalt von Leerzeichen nur bei einem Teil der möglichen Inhaltsmodelle von Bedeutung. Zu diesem Teil gehören Inhaltsmodelle, die ein Singleton vom Typ **String** in beliebiger Form, eine dedizierte Sammlung mit Elementen vom Typ **String** oder eine Mischung aus **String** und anderen Typen in Listen, Sammlungen oder Wörterbüchern akzeptieren.

Auch bei Inhaltsmodellen, die Zeichenfolgen akzeptieren, gilt das folgende Standardverhalten: Verbleibende Leerzeichen werden als unbedeutend erachtet.

### Beibehalten von Leerzeichen

Leerzeichen im Quell-XAML-Code können für die spätere Darstellung auf unterschiedliche Arten beibehalten werden. Diese sind von der XAML-Leerzeichennormalisierung des XAML-Verarbeiters nicht betroffen.

`xml:space="preserve"`: Geben Sie dieses Attribut auf der Ebene des Elements an, für das die Leerzeichen erhalten bleiben sollen. Beachten Sie, dass dadurch alle Leerzeichen erhalten bleiben, einschließlich derer, die möglicherweise von Codeeditoren oder Entwurfsoberflächen hinzugefügt werden, um Markupelemente als visuell intuitive Schachtelung auszurichten. Ob diese Leerzeichen gerendert werden ist ebenfalls wieder eine Frage des Inhaltsmodells für das Containerelement. Wir raten davon ab, `xml:space="preserve"` auf Stammebene anzugeben, da die Mehrzahl an Objektmodellen Leerzeichen in der Regel nicht als signifikant ansehen. Legen Sie das Attribut stattdessen explizit nur auf der Ebene der Elemente fest, die Leerzeichen innerhalb von Zeichenfolgen rendern oder bei denen es sich um Sammlungen handelt, in denen Leerzeichen von Bedeutung sind.

Entitäten und geschützte Leerzeichen: In XAML kann eine beliebige Unicodeentität innerhalb eines Textobjektmodells angegeben werden. Sie können dedizierte Entitäten wie geschützte Leerzeichen (in UTF-8-Codierung) verwenden. Darüber hinaus können Sie Rich-Text-Steuerelemente verwenden, die geschützte Leerzeichen unterstützen. Gehen Sie mit Bedacht vor, wenn Sie Layoutmerkmale wie Einzüge mithilfe von Entitäten simulieren, da die Laufzeitausgabe der Entitäten von einer größeren Anzahl von Faktoren abhängt als bei den allgemeinen Layoutkomponenten, beispielsweise der ordnungsgemäßen Verwendung von Bereichen und Rändern.




<!--HONumber=Jun16_HO4-->


