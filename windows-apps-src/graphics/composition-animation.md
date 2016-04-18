---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Kompositionsanimationen
description: Viele Eigenschaften von Kompositionsobjekten und Effekten können mit Keyframeanimationen und Ausdrucksanimationen animiert werden. Dadurch können sich Eigenschaften eines UI-Elements im Lauf der Zeit auf der Grundlage einer Berechnung verändern.
---
# Kompositionsanimationen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Viele Eigenschaften von Kompositionsobjekten und Effekten können mit Keyframeanimationen und Ausdrucksanimationen animiert werden. Dadurch können sich Eigenschaften eines UI-Elements im Lauf der Zeit auf der Grundlage einer Berechnung verändern. Es gibt zwei Arten von Animationen: Keyframeanimationen, dargestellt durch die [**KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706830)-Klasse, und Ausdrucksanimationen, dargestellt durch die [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817)-Klasse.

-   [Eigenschaften, die animiert werden können](./composition-animation.md#animatable_properties)
-   [Keyframeanimationen](./composition-animation.md#keyframe-animations)
    -   [Erstellen von Animationen und Keyframes](./composition-animation.md#creating-your-animation-and-Keyframes)
    -   [Keyframeeigenschaften](./composition-animation.md#keyframe-properties)
    -   [Beschleunigungsfunktionen](./composition-animation.md#easing-functions)
    -   [Starten und Beenden von Keyframeanimationen](./composition-animation.md#starting-and-stopping-keyframe-animations)
    -   [Abschlussereignisse für Animationen](./composition-animation.md#animation-completion-events)
-   [Ausdrucksanimationen](./composition-animation.md#expression-animations)
    -   [Erstellen eines Ausdrucks](./composition-animation.md#creating-your-expression)
    -   [Eigenschaftensätze](./composition-animation.md#property-sets)
    -   [Ausdruckskeyframes](./composition-animation.md#expression_keyframes)

## Eigenschaften, die animiert werden können

Die folgenden Eigenschaften lassen sich animieren, indem sie einer Keyframe- oder eine Ausdrucksanimation zugeordnet werden:

-   CompositionColorBrush.Color
-   InsetClip.BottomInset
-   InsetClip.LeftInset
-   InsetClip.RightInset
-   InsetClip.TopInset
-   Visual.AnchorPoint
-   Visual.CenterPoint
-   Visual.Offset
-   Visual.Opacity
-   Visual.Orientation
-   Visual.RotationAngle
-   Visual.RotationAxis
-   Visual.Size
-   Visual.TransformMatrix
-   CompositionPropertySet

## Keyframeanimationen

Keyframeanimationen sind zeitbasierte Animationen, die einen oder mehrere Keyframes verwenden, um anzugeben, wie der animierte Wert im Laufe der Zeit geändert werden muss. Die Frames stellen Markierungen dar, mit denen Sie den animierten Wert zu einem bestimmten Zeitpunkt festlegen können.

### Erstellen von Animationen und Keyframes

Verwenden Sie zum Erstellen einer Keyframeanimation eine der Konstruktormethoden der Kompositorklasse, die dem Strukturtyp der zu animierenden Eigenschaft entspricht.

-   [**CreateColorKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt589424)
-   [**CreateQuaternionKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt517858)
-   [**CreateScalarKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706803)
-   [**CreateVector2KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706806)
-   [**CreateVector3KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706807)
-   [**CreateVector4KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706808)

Der folgende Codeausschnitt erstellt z. B. eine Vector3-Keyframeanimation:

```cs
var animation = _compositor.CreateVector3KeyFrameAnimation();
```

Jeder Keyframe wird erstellt, indem die InsertKeyFrame-Methode einer Keyframeanimation aufgerufen und zwei Komponenten (und optional eine dritte Komponente) angegeben werden:

-   Zeit: normalisierter Fortschrittszustand des Keyframes zwischen 0,0 und 1,0
-   Wert: bestimmter Wert des Animationswerts im Zeitstatus
-   (Optional) Beschleunigungsfunktion: Funktion zum Beschreiben der Interpolation zwischen vorherigem und aktuellem Keyframe (wird später behandelt)

Beispielsweise fügt der folgende Codeausschnitt einen Keyframe in eine Vector3-Keyframeanimation ein, die mitten in der Animation ausgelöst wird:

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

### Keyframeeigenschaften

Nachdem Sie die Keyframeanimation und die einzelnen Keyframes definiert haben, können Sie mehrere Eigenschaften der Animation definieren:

-   Verzögerungszeit – Zeit vor dem Starten einer Animation, nachdem „StartAnimation()“ aufgerufen wurde
-   Dauer – Dauer der Animation
-   Iterationsverhalten – Anzahl bzw. unendliches Wiederholungsverhalten einer Animation
-   Iterationsanzahl – Anzahl der finiten Wiederholungen einer Keyframeanimation
-   Keyframeanzahl – Anzahl von Keyframes in einer bestimmten Keyframeanimation
-   Stoppverhalten – legt das Verhalten eines Animationseigenschaftswerts fest, wenn „StopAnimation“ aufgerufen wird.

Der folgende Codeausschnitt legt beispielsweise die Dauer der Animation auf fünf Sekunden fest:

```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

### Beschleunigungsfunktionen

Die Keyframebeschleunigungsfunktion, eine Kompositionsbeschleunigungsfunktion, bestimmt die Progression von Zwischenwerten vom vorherigen Keyframewert zum aktuellen Keyframewert. Wenn Sie keine Beschleunigungsfunktion für den Keyframe angeben, wird eine Standardkurve verwendet.

Es werden zwei Arten von Beschleunigungsfunktionen unterstützt:

-   Linear ([**LinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706839))
-   Kubische Bézierkurve ([**CubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706812))

Um eine Beschleunigungsfunktion zu erstellen, verwenden Sie entweder die [**CreateLinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706801) or [**CreateCubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706791)-Methode oder die [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706761)-Klasse:

```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```

Hinweis: Ein nützliches Tool für das Generieren der Punkte für die kubische Bézierkurve finden Sie hier.

Um dem Keyframe die Beschleunigungsfunktion hinzuzufügen, fügen Sie beim Einfügen in die Animation dem Keyframe einfach den dritten Parameter hinzu:

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

### Starten und Beenden von Keyframeanimationen

Nachdem Sie die Animation, Keyframes und Eigenschaften definiert haben, können Sie die Animation mit der Eigenschaft verbinden, die Sie animieren möchten. Um die Animation mit der zu animierenden Eigenschaft zu verbinden, rufen Sie [**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840) für das [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekt auf, zu dem die Eigenschaft gehört.

Die allgemeine Syntax lautet wie im folgenden Beispiel:

```cs
targetVisual.StartAnimation(“Offset”, animation);
```

Nach Sie die Animation gestartet haben, können Sie sie auch wieder anhalten und trennen. Dies geschieht mithilfe der [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841)-Methode. Geben Sie die Eigenschaft an, deren Animation Sie beenden möchten.

Beispiel:

```cs
targetVisual.StopAnimation(“Offset”);
```

### Abschlussereignisse für Animationen

Keyframeanimationen haben ein definiertes Ende – im Gegensatz zu Ausdrucksanimationen, die bedingt sind. Entwickler können mit Stapeln bestimmen, wann Gruppen oder einzelne Keyframeanimationen abgeschlossen sind. Stapel können abhängig vom Objekttyp des Stapels erstellt oder abgerufen werden. Sie werden verwendet, um den Endzustand von Animationen zu aggregieren. Bewertete Stapel werden erstellt, während Commit-Stapel abgerufen werden. Die Verwendung der einzelnen Stapel wird später behandelt.

Ein Stapelabschlussereignis wird ausgelöst, wenn alle Animationen im Stapel abgeschlossen sind. Die Zeitspanne bis zum Auslösen eines Stapelabschlussereignisses richtet sich nach der längsten oder der am meisten verzögerten Animation im Stapel. Das Aggregieren von Endzuständen ist hilfreich, wenn Sie wissen müssen, wann Gruppen bestimmter Animationen abgeschlossen sind, um andere Aufgaben zu planen.

### Bewertete Stapel

Um eine bestimmte Gruppe oder eine einzelne Animation zu aggregieren, müssen Sie zunächst einen bewerteten Stapel erstellen, indem Sie [**CreateScopedBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589425) aufrufen.

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
```

Nach dem Erstellen eines bewerteten Stapels werden alle gestarteten Animationen aggregiert, bis der Stapel explizit angehalten oder mit [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) or [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) beendet wird.

Durch Aufrufen von [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) wird die Aggregation der Endzustände von Animationen angehalten, bis [**Resume**](https://msdn.microsoft.com/library/windows/apps/Mt590847) wird. So können Sie Inhalte aus einem bestimmten Stapel explizit ausschließen.

```cs
myScopedBatch.Suspend();
```

Zum Beenden des Stapels rufen Sie [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) auf. Wenn „End“ nicht aufgerufen wird, bleibt der Stapel geöffnet und erfasst weiterhin Objekte.

```cs
myScopedBatch.End();
```

Wenn Sie wissen möchten, wann eine bestimmte Animation beendet ist, erstellen Sie einen bewerteten Stapel, starten die Animation und beenden den Stapel.

Beispiel:

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation(“Opacity”, myAnimation);
myScopedBatch.End();
```

### Commit-Stapel

Bei einem Commit-Stapel handelt es sich um einen Stapel, der während des Commit-Zyklus implizit erstellt wird. Der aktuelle Commit-Stapel kann zu einem beliebigen Zeitpunkt während des Commit-Zyklus durch Aufrufen von [**GetCommitBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589430) abgerufen werden. Der Commit-Zyklus ist die Zeitspanne zwischen Updates des Kompositors. Updates werden in die Warteschlange gestellt, bis das System zum Verarbeiten der Änderungen und Zeichnen von Bits auf dem Bildschirm bereit ist. Titel steuern Commits nicht. Der Commit-Stapel aggregiert alle Objekte innerhalb des Commit-Zyklus, bevor und nachdem „GetCommitBatch“ aufgerufen wurde. Pro Commit-Zyklus gibt es nur einen Commit-Batch, daher muss [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) für den Commit-Stapel nicht explizit aufgerufen werden.

```cs
myCommitBatch = _compositor.GetCommitBatch(CompositionBatchTypes.Animation);
```

### Stapelzustände

Um festzustellen, ob ein Stapel zum Aggregieren von Animationen geöffnet ist, verwenden Sie die Eigenschaft **IsActive**. Wenn die **IsEnded**-Eigenschaft „true“ zurückgibt, können Sie diesem Stapel eine Animation hinzufügen. Bewertete Stapel sind „beendet“, wenn sie explizit durch den Aufruf der [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844)-Methode beendet wurden. Commit-Stapel sind beendet, wenn der aktuelle Commit-Zyklus beendet wurde.

## Ausdrucksanimationen

Ausdrucksanimationen sind Animationen, die mit einem mathematischen Ausdruck angeben, wie der animierte Wert für jeden Frame berechnet werden soll. Die Ausdruckssprache selbst kann auf Eigenschaften von anderen Kompositionsobjekten verweisen. Im Gegensatz zu Keyframeanimationen basieren Ausdrücke nicht auf Zeit und werden (falls erforderlich) pro Frame verarbeitet. Ausdrücke können nützlich sein, um komplexere Benutzeroberflächen zu beschreiben, die vom Kompositionsmodul verarbeitet werden können, z. B. „Sticky Headers“ und Parallax.

### Erstellen eines Ausdrucks

Um einen Ausdruck zu erstellen, rufen Sie für das Kompositorobjekt [**CreateExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt187002) auf und geben den gewünschten Ausdruck an:

```cs
var expression = _compositor.CreateExpressionAnimation(“INSERT_EXPRESSION_STRING”)
```

### Operatoren, Rangfolge und Orientierung

Die Ausdruckssprache unterstützt die folgenden Operatoren und entspricht gemäß der Definition in der C#-Sprachspezifikation der Operatorrangfolge und -assoziativität:

-   Unär (-)
-   Multiplikativ (\* /)
-   Additiv (-+)

Die Ausdruckssprache unterstützt auch das Konzept von mathematischen Vorgängen auf Komponentenbasis. Diese Operatoren für Member-Zugriff (x.y) werden nur für „Grundtypen“ (Vektor und Matrizen) unterstützt. Beispiel:

```cs
Offset.x + 5.0
```

### Eigenschaftenparameter

Eine leistungsstarke Komponente der Ausdruckssprache ist die Verwendung von Parametern als Variablen im Ausdruck. Die Ausdruckszeichenfolge kann Parameter enthalten, die entweder durch einen Konstantenwert ersetzt werden oder bei der Berechnung als Verweise auf den Eigenschaftswert eines anderen Objekts dienen. Beispiel:

```cs
ChildVisual.Offset.X / ParentVisual.Offset.Y
```

In diesem Beispiel sind „ChildOffset“ und „ParentOffset“ Parameter, die die Offset-Eigenschaft von zwei visuellen Elementen darstellen. Sie definieren die Parameter mit der **Set\*Parameter**-Methode der [**CompositionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706708)-Klasse:

-   Einen Vektorparameter erstellen Sie mit [**SetVector2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector2parameter.aspx), [**SetVector3Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector3parameter.aspx) oder [**SetVector4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setvector4parameter).
-   Einen Matrixparameter erstellen Sie mit [**SetMatrix3x2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix3x2parameter.aspx) oder [**SetMatrix4x4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix4x4parameter).
-   Einen skalaren Parameter erstellen Sie mit [**SetScalarParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setscalarparameter).
-   Einen Farbparameter erstellen Sie mit [**SetColorParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setcolorparameter).
-   Einen Quaternionparameter erstellen Sie mit [**SetQuaternionParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setquaternionparameter).
-   Einen Referenzparameter erstellen Sie mit [**SetReferenceParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setreferenceparameter).

In der oben genannten Ausdruckszeichenfolge müssten zwei Parameter erstellt werden, um die beiden visuellen Strukturen zu definieren:

```cs
Expression.SetReferenceParameter(“ChildVisual”, childVisual);
Expression.SetReferenceParameter(“ParentVisual”, parentVisual);
```

### Ausdruckshilfsfunktionen

Zusätzlich zum Zugriff auf die Operatoren und Eigenschaftenparameter haben Sie Zugriff auf die umfassende Hilfsfunktionsbibliothek der Ausdruckssprache. Die vollständige Liste der Hilfsfunktionen finden Sie im Abschnitt „Anmerkungen“ der [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817)-Klasse. Einige davon sind hier aufgeführt:

-   Max (Scalar value1, Scalar value2)
-   Scale (Vector3 value, Scalar factor)
-   Transform (Vector2 value, Matrix 3x2 matrix)

Hier ist ein Beispiel für eine komplexere Ausdruckszeichenfolge, die die Hilfsfunktion „Clamp“ verwendet:

```cs
Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)
```

### Starten und Anhalten der Ausdrucksanimation

Zum Starten und Beenden von Ausdrucksanimationen verwenden Sie die gleichen Funktionen ([**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840), [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841)) wie für Keyframeanimationen. Hinweis: Ausdrucksanimationen werden weiterhin ausgeführt, bis sie mit **StopAnimation** beendet werden. Im Gegensatz zu Keyframeanimationen ist ihre Dauer nicht begrenzt.

### Eigenschaftensätze

Sie können nicht nur auf Eigenschaften von anderen Kompositionsobjekten verweisen, sondern haben auch die Möglichkeit, eigene Eigenschaftswerte zu erstellen und zu speichern, die über mehrere Animationen hinweg verwendet werden können. Eigenschaftensätze werden durch die [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772)-Klasse dargestellt. Mithilfe der Eigenschaftensätze können Sie eine Eigenschaft mit einem Wert erstellen und später in einem Ausdruck auf sie verweisen. Sie können sie aber auch als Ziel einer Keyframeanimation einsetzen.

Verwenden Sie zum Erstellen eines Eigenschaftensatzes die [**CreatePropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706802)-Methode der Kompositorklasse. Beispiel:

```cs
_sharedProperties = _compositor.CreatePropertySet();
```

Nachdem Sie den Eigenschaftensatz erstellt haben, können Sie eine Eigenschaft und einen Wert mithilfe einer der **Insert\***-Methoden von [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772) hinzufügen. Beispiel:

```cs
_sharedProperties.InsertVector3(“NewOffset”, offset);
```

Nachdem Sie Ihre Ausdrucksanimation erstellt haben, können Sie auf Eigenschaften aus dem Eigenschaftensatz in der Ausdruckszeichenfolge verweisen, indem Sie einen Verweisparameter verwenden. Beispiel:

```cs
var expression = _compositor.CreateExpressionAnimation(“sharedProperties.NewOffset”);
expression.SetReferenceParameter(“sharedProperties”, _sharedProperties);
```

### Ausdruckskeyframes

Weiter oben in diesem Dokument wird beschrieben, wie Keyframeanimationen und ihre jeweiligen Keyframes erstellt werden. Zusätzlich zu den herkömmlichen Keyframes mit einer normalisierten Zeit- und Wertkomponente können Sie auch Ausdruckskeyframes definieren.

Statt einen expliziten Wert für einen bestimmten Zeitpunkt im Keyframe zu definieren, können wir mithilfe der Ausdruckssyntax auch beschreiben, wie der Wert an diesem speziellen Punkt auf der Zeitachse des Keyframes berechnet werden soll. Sie können in Keyframeanimationen jederzeit Ausdruckskeyframes mit einfachen Keyframes kombinieren.

### Erstellen von Ausdruckskeyframes

Ähnlich wie herkömmliche Keyframes werden Ausdruckskeyframes durch Angabe von zwei Komponenten gebildet:

-   Zeit: normalisierter Zeitzustand des Keyframes zwischen 0,0 und 1,0
-   Wert: Die Ausdruckszeichenfolge, mit der beschrieben wird, wie der Wert berechnet werden soll

Der folgende Codeausschnitt verwendet beispielsweise eine Kombination aus regulären Keyframes und Ausdruckskeyframes:

```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, “VisualBOffset.X / VisualAOffset.Y”);
animation.InsertKeyFrame(1.00f, 0.8f);
```

### Verwenden von aktuellen und Ausgangswerten

In der Ausdruckssprache ist es möglich, auf den aktuellen Wert und den Ausgangswert einer Animation zu verweisen. Diese Werte werden in der Ausdruckssprache mit dem Präfix „this“ versehen:

-   this.CurrentValue
-   this.StartingValue

Ein Anwendungsbeispiel in einem Ausdruckskeyframe:

```cs
animation.InsertExpressionKeyFrame(0.0f, “this.CurrentValue + delta”);
```

### Bedingte Ausdrücke

Neben der Unterstützung von grundlegenden mathematischen Ausdrücken können Sie einen bedingten Ausdrucks auch mit einem ternären Operator definieren:

```cs
(condition ? first_expression : second_expression)
```

In den Ausdrücken selbst werden die folgenden bedingten Operatoren in der Bedingungsanweisung unterstützt:

-   Ist gleich (==)
-   Ist ungleich (!=)
-   Kleiner als (<)
-   Kleiner oder gleich (<=)
-   Größer als (>)
-   Größer oder gleich (>=)

Und nicht zuletzt werden auch die folgenden Konjunktionen als Operatoren oder Funktionen in der Anweisungsbedingung unterstützt:

-   Not: ! / Not(bool1)
-   And: && / And(bool1, bool2)
-   Or: || / Or(bool1, bool2)

Der folgende Codeausschnitt zeigt ein Beispiel für die Verwendung von konditionellen Abschnitten in einem Ausdruck:

```cs
var expression = _compositor.CreateExpressionAnimation(“target.Offset.x > 50 ? 0.0f +   (target.Offset.x / parent.Offset.x) : 1.0f”);
```

 

 






<!--HONumber=Mar16_HO1-->


