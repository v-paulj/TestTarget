---
author: jwmsft
description: Informieren Sie sich über die Verschieben- und Zeichnen-Befehle (eine Minisprache), mit denen Sie Pfadgeometrien als XAML-Attributwert angeben können.
title: Syntax für die Verschieben- und Zeichnen-Befehle
ms.assetid: 7772BC3E-A631-46FF-9940-3DD5B9D0E0D9
---

# Syntax für die Verschieben- und Zeichnen-Befehle

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Informieren Sie sich über die Verschieben- und Zeichnen-Befehle (eine Minisprache), mit denen Sie Pfadgeometrien als XAML-Attributwert angeben können. Verschieben- und Zeichnen-Befehle werden von vielen Design- und Grafiktools, die eine Vektorgrafik oder Form ausgeben können, als Serialisierungs- und Austauschformat verwendet.

## Eigenschaften, die Verschieben- und Zeichnen-Befehlszeichenfolgen verwenden

Die Syntax für die Verschieben- und Zeichnen-Befehle wird von einem internen Typkonverter für XAML verwendet, der die Befehle analysiert und eine Laufzeit-Grafikdarstellung erzeugt. Diese Darstellung ist im Grunde ein fertiger Satz von Vektoren, die zur Anzeige bereit sind. Mit den Vektoren selbst sind die Darstellungsdetails noch nicht vollständig. Sie müssen weitere Werte für die Elemente festlegen. Für ein [**Path**](https://msdn.microsoft.com/library/windows/apps/br243355)-Objekt benötigen Sie auch Werte für [**Fill**](https://msdn.microsoft.com/library/windows/apps/br243378), [**Stroke**](https://msdn.microsoft.com/library/windows/apps/br243383) und andere Eigenschaften. Anschließend muss der **Path** mit der visuellen Struktur verbunden werden. Für ein [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722)-Objekt legen Sie die [**Foreground**](https://msdn.microsoft.com/library/windows/apps/dn251974)-Eigenschaft fest.

Die Windows-Runtime enthält zwei Eigenschaften, die eine Zeichenfolge verwenden können, die Verschieben- und Zeichnen-Befehle darstellt: [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356) und [**PathIcon.Data**](https://msdn.microsoft.com/library/windows/apps/dn252723). Wenn Sie eine dieser Eigenschaften festlegen, indem Sie Verschieben- und Zeichnen-Befehle angeben, legen Sie dafür in der Regel einen XAML-Attributwert sowie weitere erforderliche Attribute dieses Elements fest. Ohne auf die Einzelheiten einzugehen, sieht das Ganze so aus:

```xml
<Path x:Name="Arrow" Fill="White" Height="11" Width="9.67"
  Data="M4.12,0 L9.67,5.47 L4.12,10.94 L0,10.88 L5.56,5.47 L0,0.06" />
```

[
              **PathGeometry.Figures**
            ](https://msdn.microsoft.com/library/windows/apps/br210169) kann ebenfalls Verschieben- und Zeichnen-Befehle verwenden. Sie können ein [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)-Objekt, das Verschieben- und Zeichnen-Befehle verwendet, mit anderen [**Geometry**](https://msdn.microsoft.com/library/windows/apps/br210041)-Typen in einem [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/br210057)-Objekt kombinieren, das Sie dann als Wert für [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356) nutzen. Weitaus gängiger ist aber die Verwendung von Verschieben- und Zeichnen-Befehlen für durch Attribute definierte Daten.

## Verwenden von Verschieben- und Zeichnen-Befehlen oder einer **PathGeometry**-Klasse

Für Windows-Runtime-XAML erzeugen die Verschieben- und Zeichnen-Befehle eine [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)-Klasse mit einem einzigen [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/br210143)-Objekt, das über einen [**Figures**](https://msdn.microsoft.com/library/windows/apps/br210169)-Eigenschaftswert verfügt. Jeder Zeichnen-Befehl erzeugt eine von der [**PathSegment**](https://msdn.microsoft.com/library/windows/apps/br210174)-Klasse abgeleitete Klasse in der [**Segments**](https://msdn.microsoft.com/library/windows/apps/br210164)-Collection dieses einen **PathFigure**-Objekts. Der Verschieben-Befehl ändert die [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/br210166)-Eigenschaft. Zudem wird durch das Vorhandensein eines Schließen-Befehls [**IsClosed**](https://msdn.microsoft.com/library/windows/apps/br210159) auf **true** festgelegt. In dieser Struktur können Sie dann wie in einem Objektmodell navigieren, wenn Sie zur Laufzeit die **Data**-Werte untersuchen.

## Grundlegende Syntax

Hier ein Überblick der Syntax für Verschieben- und Zeichnen-Befehle:

1.  Beginnen Sie mit einer optionalen Füllregel. Normalerweise geben Sie diese Füllregel nur an, wenn Sie den **EvenOdd**-Standard nicht verwenden möchten. (**EvenOdd** wird später erläutert.)
2.  Geben Sie genau einen Verschieben-Befehl an.
3.  Geben Sie mindestens einen Zeichnen-Befehl an.
4.  Geben Sie einen Schließen-Befehl an. Sie können den Schließen-Befehl weglassen, aber dann bleibt die Figur geöffnet (das wäre ungewöhnlich).

Allgemeine Regeln für diese Syntax:

-   Jeder Befehl wird durch genau einen Buchstaben dargestellt.
-   Dabei kann es sich um einen Groß- oder Kleinbuchstaben handeln. Die Groß-/Kleinschreibung spielt eine Rolle. Darauf gehen wir noch ein.
-   Auf jeden Befehl mit Ausnahme des Schließen-Befehls folgt normalerweise mindestens eine Zahl.
-   Wenn Sie für einen Befehl mehrere Zahlen angeben, trennen Sie diese durch Komma oder Leerzeichen.

**\[**
            _fillRule_
            **\]**
            _moveCommand_
            _drawCommand_
            **\[**
            _drawCommand_
            **\*\]**
            **\[**
            _closeCommand_
            **\]**

Viele der Zeichnen-Befehle verwenden Punkte, für die Sie einen _x,y_-Wert angeben. Wenn Sie einen \*_points_-Platzhalter sehen, können Sie davon ausgehen, dass Sie für den _x,y_-Wert eines Punkts zwei Dezimalwerte angeben müssen.

Leerzeichen können bei eindeutigen Ergebnissen häufig weggelassen werden. Tatsächlich können Sie alle Leerzeichen weglassen, wenn Sie Kommas als Trennzeichen für alle Zahlengruppen (Punkte und Größe) verwenden. Diese Verwendung ist zum Beispiel gültig: `F1M0,58L2,56L6,60L13,51L15,53L6,64z`. Typischer ist allerdings die Verwendung von Leerzeichen zwischen Befehlen, um die Übersichtlichkeit zu verbessern.

Verwenden Sie als Dezimalzeichen für Dezimalzahlen kein Komma. Die Befehlszeichenfolge wird von XAML interpretiert und berücksichtigt keine länderspezifischen Konventionen für die Zahlenformatierung, die von den Konventionen im Gebietsschema **en-us** abweichen.

## Einzelheiten zur Syntax

**Füllregel**

Es gibt zwei mögliche Werte für die optionale Füllregel: **F0** oder **F1**. (Das **F** wird immer großgeschrieben.) **F0** ist der Standardwert. Er erzeugt das **EvenOdd**-Füllverhalten, sodass Sie dieses normalerweise nicht angeben. Verwenden Sie **F1**, um das Füllverhalten für **Nonzero** abzurufen. Diese Füllwerte sind an die Werte der [**FillRule**](https://msdn.microsoft.com/library/windows/apps/br210030)-Aufzählung angepasst.

**Verschieben-Befehl**

Gibt den Anfangspunkt einer neuen Figur an.

| Syntax |
|--------|
| `M ` _startPoint_ <br/>- oder -<br/>`m` _startPoint_|

| Benennung | Beschreibung |
|------|-------------|
| _startPoint_ | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/>Der Anfangspunkt einer neuen Figur|

Der Großbuchstabe **M** gibt an, dass *startPoint* eine absolute Koordinate ist. Der Kleinbuchstabe **m** gibt an, dass *startPoint* ein Offset zum vorherigen Punkt ist. Wenn kein vorheriger Punkt vorhanden ist, wird (0,0) angegeben.

**Hinweis**  Sie können nach dem Verschieben-Befehl mehrere Points angeben. Es wird eine Linie zu diesen Punkten gezeichnet, als hätten Sie den Linienbefehl angegeben. Dieser Stil wird aber nicht empfohlen, verwenden Sie stattdessen den speziellen Linienbefehl.

**Zeichnen-Befehle**

Ein Zeichnen-Befehl kann aus mehreren Formbefehlen bestehen: Linie, horizontale Linie, vertikale Linie, kubische Bézierkurve, quadratische Bézierkurve, glatte Bézierkurve, glatte quadratische Bézierkurve und Ellipsenbogen.

Bei allen Zeichnen-Befehlen spielt die Groß-/Kleinschreibung eine Rolle. Großbuchstaben geben absolute Koordinaten an und Kleinbuchstaben geben relative Koordinaten zum vorherigen Befehl an.

Die Kontrollpunkte für ein Segment sind relativ zum Endpunkt des vorherigen Segments. Wenn Sie nacheinander mehrere Befehle des gleichen Typs eingeben, müssen Sie die Befehle nicht doppelt eingeben. So ist zum Beispiel `L 100,200 300,400` gleichwertig mit `L 100,200 L 300,400`.

**Linienbefehl**

Erstellt eine gerade Linie zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. `l 20 30` und `L 20,30` sind Beispiele für gültige Linienbefehle. Definiert das Äquivalent eines [**LineGeometry**](https://msdn.microsoft.com/library/windows/apps/br210117)-Objekts.

| Syntax |
|--------|
| `L` _endPoint_ <br/>- oder -<br/>`l` _endPoint_ |

| Benennung | Beschreibung |
|------|-------------|
| endPoint | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/>Der Endpunkt der Linie|

**Befehl für eine horizontale Linie**

Erstellt eine horizontale Linie zwischen dem aktuellen Punkt und der angegebenen x-Koordinate. `H 90` ist ein Beispiel für einen gültigen Befehl für eine horizontale Linie.

| Syntax |
|--------|
| `H ` _x_ <br/> - oder - <br/>`h ` _x_ |

| Benennung | Beschreibung |
|------|-------------|
| x | [**Double**](T:System.Double) <br/> Die x-Koordinate am Endpunkt der Linie |

**Befehl für eine vertikale Linie**

Erstellt eine vertikale Linie zwischen dem aktuellen Punkt und der angegebenen y-Koordinate. `v 90` ist ein Beispiel für einen gültigen Befehl für eine vertikale Linie.

| Syntax |
|--------|
| `V ` _y_ <br/> - oder - <br/> `v ` _y_ |

| Benennung | Beschreibung |
|------|-------------|
| *y* | [**Double**](T:System.Double) <br/> Die y-Koordinate am Endpunkt der Linie |

**Befehl für eine kubische Bézierkurve**

Erstellt eine kubische Bézierkurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Dabei werden die beiden angegebenen Kontrollpunkte (*controlPoint1* und *controlPoint2*) verwendet. `C 100,200 200,400 300,200` ist ein Beispiel für einen gültigen Kurvenbefehl. Definiert das Äquivalent eines [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)-Objekts mit einem [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/br228068)-Objekt.

| Syntax |
|--------|
| `C ` *controlPoint1*
            *controlPoint2*
            *endPoint* <br/> - oder - <br/> `c ` *controlPoint1*
            *controlPoint2*
            *endPoint* |

| Benennung | Beschreibung |
|------|-------------|
| *controlPoint1* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Der erste Kontrollpunkt der Kurve, der die Anfangstangente der Kurve bestimmt. |
| *controlPoint2* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Der zweite Kontrollpunkt der Kurve, der die Endtangente der Kurve bestimmt. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Der Punkt, zu dem die Kurve gezeichnet wird. | 

**Befehl für eine quadratische Bézierkurve**

Erstellt eine quadratische Bézierkurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Dabei wird der angegebene Kontrollpunkt (*controlPoint*) verwendet. `q 100,200 300,200` ist ein Beispiel für einen gültigen Befehl für eine quadratische Bézierkurve. Definiert das Äquivalent eines [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)-Objekts mit einem [**QuadraticBezierSegment**](https://msdn.microsoft.com/library/windows/apps/br210249)-Objekt.

| Syntax |
|--------|
| `Q ` *controlPoint endPoint* <br/> - oder - <br/> `q ` *controlPoint endPoint* |

| Benennung | Beschreibung |
|------|-------------|
| *controlPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Der Kontrollpunkt der Kurve, der die Anfangs- und Endtangente der Kurve bestimmt. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> Der Punkt, zu dem die Kurve gezeichnet wird. |

**Befehl für eine glatte kubische Bézierkurve**

Erstellt eine kubische Bézierkurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Als erster Kontrollpunkt wird die Spiegelung des zweiten Kontrollpunkts des vorherigen Befehls relativ zum aktuellen Punkt angenommen. Wenn kein vorheriger Befehl vorhanden ist oder der vorherige Befehl kein Befehl für eine kubische Bézierkurve oder eine glatte kubische Bézierkurve war, können Sie annehmen, dass der erste Kontrollpunkt mit dem aktuellen Punkt deckungsgleich ist. Der zweite Kontrollpunkt – der Kontrollpunkt für das Ende der Kurve – wird mit *controlPoint2* angegeben. `S 100,200 200,300` ist zum Beispiel ein gültiger Befehl für eine glatte kubische Bézierkurve. Dieser Befehl definiert das Äquivalent eines [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)-Objekts mit einem [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/br228068)-Objekt, wobei ein vorhergehendes Kurvensegment vorhanden war.

| Syntax |
|--------|
| `S` *controlPoint2*
            *endPoint* <br/> - oder - <br/>`s` *controlPoint2 endPoint* |

| Benennung | Beschreibung |
|------|-------------|
| *controlPoint2* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Der Kontrollpunkt der Kurve, der die Endtangente der Kurve bestimmt. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> Der Punkt, zu dem die Kurve gezeichnet wird. |

**Befehl für eine glatte quadratische Bézierkurve**

Erstellt eine quadratische Bézierkurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Als Kontrollpunkt wird die Spiegelung des Kontrollpunkts des vorherigen Befehls relativ zum aktuellen Punkt angenommen. Wenn kein vorheriger Befehl vorhanden ist oder der vorherige Befehl kein Befehl für eine quadratische Bézierkurve oder eine glatte quadratische Bézierkurve war, ist der Kontrollpunkt mit dem aktuellen Punkt deckungsgleich. Dieser Befehl definiert das Äquivalent eines [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)-Objekts mit einem [**QuadraticBezierSegment**](https://msdn.microsoft.com/library/windows/apps/br210249)-Objekt, wobei ein vorhergehendes Kurvensegment vorhanden war.

| Syntax |
|--------|
| `T` *controlPoint*
            *endPoint* <br/> - oder - <br/> `t` *controlPoint*
            *endPoint* |

| Benennung | Beschreibung |
|------|-------------|
| *controlPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> Der Kontrollpunkt der Kurve, der die Anfangstangente der Kurve bestimmt. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> Der Punkt, zu dem die Kurve gezeichnet wird. |

**Befehl für einen Ellipsenbogen**

Erstellt einen Ellipsenbogen zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Definiert das Äquivalent eines [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)-Objekts mit einem [**ArcSegment**](https://msdn.microsoft.com/library/windows/apps/br228054)-Objekt.

| Syntax |
|--------|
| `A ` *size*
            *rotationAngle*
            *isLargeArcFlag*
            *sweepDirectionFlag*
            *endPoint* <br/> - oder - <br/>`a ` *sizerotationAngleisLargeArcFlagsweepDirectionFlagendPoint* |

| Benennung | Beschreibung |
|------|-------------|
| *size* | [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)<br/>Der x-Radius und y-Radius des Bogens |
| *rotationAngle* | [**Double**](T:System.Double) <br/> Die Drehung der Ellipse in Grad |
| *isLargeArcFlag* | Legen Sie „1“ fest, wenn der Winkel des Bogens mindestens 180 Grad entsprechen soll. Legen Sie anderenfalls „0“ fest. |
| *sweepDirectionFlag* | Legen Sie „1“ fest, wenn der Bogen in Richtung eines positiven Winkels gezeichnet wird. Legen Sie anderenfalls „0“ fest. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Der Punkt, zu dem die Kurve gezeichnet wird.|
 
**Schließen-Befehl**

Beendet die aktuelle Figur und erstellt eine Linie, die den aktuellen Punkt mit dem Anfangspunkt der Figur verbindet. Dieser Befehl erstellt eine Linienverbindung (Ecke) zwischen dem letzten Segment und dem ersten Segment der Figur.

| Syntax |
|--------|
| `Z` <br/> – oder – <br/> `z ` |

**Syntax für Punkte**

Beschreibt die x-Koordinate und y-Koordinate eines Punkts. Weitere Informationen finden Sie auch unter [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870).

| Syntax |
|--------|
| *x*,*y*<br/> - oder - <br/>*x*
            *y* |

| Benennung | Beschreibung |
|------|-------------|
| *x* | [**Double**](T:System.Double) <br/> Die x-Koordinate des Punkts |
| *y* | [**Double**](T:System.Double) <br/> Die y-Koordinate des Punkts |

**Weitere Anmerkungen**

Anstelle eines standardmäßigen numerischen Werts können Sie auch die folgenden speziellen Werte verwenden. Bei diesen Werten wird zwischen Groß-/Kleinschreibung unterschieden.

-   **Infinity**: repräsentiert **PositiveInfinity**.
-   **\-Infinity**: repräsentiert **NegativeInfinity**.
-   **NaN**: repräsentiert **NaN**.

Anstelle von Dezimalzahlen oder Ganzzahlen können Sie die wissenschaftliche Notation verwenden. `+1.e17` ist zum Beispiel ein gültiger Wert.

## Designtools zum Erzeugen von Verschieben- und Zeichnen-Befehlen

Mit dem **Pen**-Tool und anderen Zeichentools in Blend for Microsoft Visual Studio 2013 erzeugen Sie normalerweise ein [**Path**](https://msdn.microsoft.com/library/windows/apps/br243355)-Objekt mit Verschieben- und Zeichnen-Befehlen.

Möglicherweise sehen Sie vorhandene Daten für Verschieben- und Zeichnen-Befehle in einigen Steuerelementkomponenten, die in den Standardvorlagen für Steuerelemente in Windows-Runtime-XAML definiert sind. So verwenden zum Beispiel einige Steuerelemente ein [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722)-Objekt, dessen Daten als Verschieben- und Zeichnen-Befehle definiert sind.

Für andere häufig verwendete Vektorgrafik-Designtools, die den Vektor in XAML-Form ausgeben können, sind Exporter oder Plug-Ins verfügbar. Diese erstellen gewöhnlich [**Path**](https://msdn.microsoft.com/library/windows/apps/br243355)-Objekte in einem Layoutcontainer mit Verschieben- und Zeichnen-Befehlen für die [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356)-Eigenschaft. XAML kann mehrere **Path**-Elemente enthalten, sodass verschiedene Pinsel angewendet werden können. Diese Exporter oder Plug-Ins wurden ursprünglich für Windows Presentation Foundation (WPF)XAML geschrieben, aber dieser Aspekt der XAML-Syntax ist mit Windows-Runtime-XAML identisch. Oft können Sie XAML-Abschnitte aus einem Exporter verwenden und direkt in eine Windows-Runtime-XAML-Seite einfügen. (Es ist aber nicht möglich, einen **RadialGradientBrush**-Pinsel zu verwenden, wenn dieser Bestandteil der konvertierten XAML war, da Windows-Runtime-XAML diesen Pinsel nicht unterstützt.)

## Verwandte Themen

* [Zeichnen von Formen](https://msdn.microsoft.com/library/windows/apps/mt280380)
* [Verwenden von Pinseln](https://msdn.microsoft.com/library/windows/apps/mt280383)
* [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356)
* [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722)



<!--HONumber=May16_HO2-->


