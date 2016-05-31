---
author: jwmsft
description: Dient zum eindeutigen Identifizieren von Objektelementen für den Zugriff auf das instanziierte Objekt aus CodeBehind- oder allgemeinem Code.
title: xName-Attribut
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
---

# x:Name-Attribut

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Dient zum eindeutigen Identifizieren von Objektelementen für den Zugriff auf das instanziierte Objekt aus CodeBehind oder allgemeinem Code. Nach Anwendung auf ein zugrunde liegendes Programmiermodell kann **x:Name** als Äquivalent der Variablen mit einem Objektverweis betrachtet werden (gemäß Rückgabe von einem Konstruktor).

## XAML-Attributsyntax

``` syntax
<object x:Name="XAMLNameValue".../>
```

## XAML-Werte

| Benennung | Beschreibung |
|------|-------------|
| XAMLNameValue | Eine Zeichenfolge, die den Einschränkungen derXamlName-Grammatik entspricht. |

##  XamlName-Grammatik

Im Anschluss finden Sie die maßgebende Grammatik für eine Zeichenfolge, die in dieser XAML-Implementierung als Schlüssel verwendet wird:

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Die Zeichen sind auf den unteren ASCII-Bereich beschränkt – genauer: auf Groß- und Kleinbuchstaben des lateinischen Alphabets sowie auf Ziffern und den Unterstrich (\_).
-   Der Unicode-Zeichenbereich wird nicht unterstützt.
-   Ein Name darf nicht mit einer Ziffer beginnen. Einige Toolimplementierungen stellen einer Zeichenfolge einen Unterstrich (\_) voran, wenn der Benutzer als erstes Zeichen eine Ziffer angibt, oder das Tool generiert automatisch **x:Name**-Werte auf Grundlage anderer Werte, die Ziffern enthalten.

## Hinweise

Das angegebene **x:Name**-Attribut wird zum Namen eines Felds, das bei der XAML-Verarbeitung im zugrunde liegenden Code erstellt wird. Dieses Feld enthält einen Verweis auf das Objekt. Die Erstellung dieses Felds erfolgt im Rahmen der MSBuild-Zielschritte. Im Rahmen dieser Schritte erfolgt auch der Beitritt zu den partiellen Klassen für eine XAML-Datei und das zugehörige CodeBehind. Dieses Verhalten ist nicht zwingend XAML-bedingt. Ausschlaggebend ist vielmehr die jeweilige Implementierung, die von der UWP-Programmierung (Universelle Windows-Plattform) für XAML angewendet wird, um **x:Name** in den entsprechenden Programmier- und Anwendungsmodellen zu verwenden.

Jedes definierte **x:Name**-Attribut muss innerhalb eines XAML-Namescopes eindeutig sein. Im Allgemeinen wird ein XAML-Namescope auf der Stammelementebene einer geladenen Seite definiert und enthält alle Elemente unter diesem Element auf einer einzelnen XAML-Seite. Zusätzliche XAML-Namescopes werden von beliebigen, ebenfalls auf dieser Seite definierten Steuerelement- oder Datenvorlagen definiert. Zur Laufzeit wird ein weiterer XAML-Namescope für den Stamm der aus einer angewendeten Steuerelementvorlage erstellten Objektstruktur und auch von Objektstrukturen, die aus einem Aufruf von [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) erstellt werden, definiert. Weitere Informationen finden Sie unter [XAML-Namescopes](xaml-namescopes.md).

Entwurfstools erzeugen oft automatisch **x:Name**-Werte für Elemente, wenn diese auf der Entwurfsoberfläche eingeführt werden. Welches Schema für die automatische Generierung verwendet wird, hängt zwar davon ab, welchen Designer Sie verwenden, ein häufig verwendetes Schema ist jedoch die Generierung einer Zeichenfolge, die mit dem zugrunde liegenden Klassennamen des Elements beginnt und im Anschluss daran mit einer laufenden ganzen Zahl versehen wird. Wenn Sie also beispielsweise das erste [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)-Element im Designer einführen, besitzt dieses Element in XAML unter Umständen für das **x:Name**-Attribut den Wert „Button1“.

**x:Name** kann nicht in der XAML-Eigenschaftselementsyntax oder in Code mit [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) festgelegt werden. **x:Name** kann nur über die XAML-Attributsyntax für Elemente festgelegt werden.

**Hinweis**  Speziell bei C++-/CX-Apps wird für das Stammelement einer XAML-Datei oder -Seite kein Sicherungsfeld für einen **x:Name**-Verweis erstellt. Wenn Sie in C++-CodeBehind auf das Stammobjekt verweisen müssen, verwenden Sie andere APIs oder eine Strukturtraversierung. Sie können beispielsweise erst [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) für ein bekanntes benanntes Unterelement und anschließend [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739) aufrufen.

### x:Name und andere Name-Eigenschaften

Einige in UWP-XAML verwendete Typen verfügen auch über die Eigenschaft **Name**. Beispiele: [**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) und [**TextElement.Name**](https://msdn.microsoft.com/library/windows/apps/hh702125).

Falls **Name** als einstellbare Eigenschaft für ein Element verfügbar ist, können **Name** und **x:Name** in XAML synonym verwendet werden. Werden allerdings beide Attribute für das gleiche Element angegeben, tritt ein Fehler auf. In einigen Fällen ist zwar eine **Name**-Eigenschaft vorhanden, diese ist jedoch schreibgeschützt (z. B. [**VisualState.Name**](https://msdn.microsoft.com/library/windows/apps/br209031)). In diesem Fällen verwenden Sie immer **Name** zum Benennen dieses Elements im XAML, und die schreibgeschützte **Name** ist für seltener verwendete Codeszenarien vorhanden.

**Hinweis**
            [
              **FrameworkElement.Name**
            ](https://msdn.microsoft.com/library/windows/apps/br208735) sollte im Allgemeinen nicht zum Ändern von Werten verwendet werden, die ursprünglich von **x:Name** festgelegt wurden. Einige Szenarien bilden jedoch Ausnahmen von dieser allgemeinen Regel. In typischen Szenarien handelt es sich bei der Erstellung und Definition von XAML-Namescopes um Vorgänge des XAML-Prozessors. Das Ändern von **FrameworkElement.Name** zur Laufzeit kann einen inkonsistenten XAML-Namescope bzw. eine inkonsistente Benennungsausrichtung privater Felder zur Folge haben, was im CodeBehind nur schwer nachvollziehbar ist.

### x:Name und x:Key

**x:Name** kann als Attribut auf Elemente innerhalb von [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) angewendet werden, um als Ersatz für das [x:Key-Attribut](x-key-attribute.md) zu dienen. (Normalerweise gilt die Regel, dass alle Elemente in einem **ResourceDictionary** ein x:Key-Attribut aufweisen müssen.) Bei [Storyboardanimationen](https://msdn.microsoft.com/library/windows/apps/mt187354) ist dies häufig der Fall. Weitere Informationen finden Sie im entsprechenden Abschnitt unter [ResourceDictionary- und XAML-Ressourcenverweise](https://msdn.microsoft.com/library/windows/apps/mt187273).



<!--HONumber=May16_HO2-->


