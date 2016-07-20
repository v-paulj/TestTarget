---
author: scottmill
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Kompositionsanimationen
description: "Viele Eigenschaften von Kompositionsobjekten und Effekten können mit Keyframeanimationen und Ausdrucksanimationen animiert werden. Dadurch können sich Eigenschaften eines UI-Elements im Laufe der Zeit oder auf der Grundlage einer Berechnung verändern."
translationtype: Human Translation
ms.sourcegitcommit: 62f0ea80940ff862d26feaa063414d95b048f685
ms.openlocfilehash: e0088692b9de10c188f15b85b1f20b98cc113517

---
# Kompositionsanimationen

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

Mit der Windows.UI.Composition WinRT-API können Sie Kompositorobjekte in einer einheitlichen API-Ebene erstellen, animieren, umwandeln und bearbeiten. Kompositionsanimationen bieten eine leistungsstarke und effiziente Möglichkeit, Animationen auf der Benutzeroberfläche Ihrer Anwendung auszuführen. Sie wurden von Grund auf neu entwickelt, um sicherzustellen, dass Ihre Animationen mit 60 F/s unabhängig vom UI-Thread ausgeführt werden, und um Ihnen die Flexibilität zu bieten, tolle Funktionen zu erstellen, die zur Steuerung von Animationen nicht nur Zeit, sondern auch Eingaben und andere Eigenschaften verwenden.
Dieses Thema bietet eine Übersicht über die verfügbaren Funktionen, mit denen Sie Eigenschaften des Kompositionsobjekts animieren können.
In diesem Dokument wird davon ausgegangen, dass Sie mit den Grundlagen der Struktur visueller Ebenen vertraut sind. Weitere Informationen [finden Sie hier](./composition-visual-tree.md). Es gibt zwei Arten von Kompositionsanimationen : **Keyframeanimationen** und **Ausdrucksanimationen**  

![](./images/composition-animation-types.png)  
   
 
##Typen von Kompositionsanimationen

            **Keyframeanimationen** bieten die herkömmlichen zeitgesteuerten *Frame-für-Frame*-Animationsfunktionen. Entwickler können explizit *Kontrollpunkte* definieren, die Werte dafür definieren, wann eine Animationseigenschaft an bestimmten Punkten in der Animationszeitachse sein muss. Was noch wichtiger ist, Sie können Beschleunigungsfunktionen (andernfalls Interpolatoren genannt) verwenden, um den Übergang zwischen diesen Kontrollpunkten zu beschreiben.  


            **Ausdrucksanimationen** sind eine neue Art von Animation, die in der visuellen Ebene mit dem Windows 10 November-Update (Build 10586) eingeführt wurde. Mit Ausdrucksanimationen soll ein Entwickler mathematischen Beziehungen zwischen visuellen Eigenschaften und einzelnen Werten erstellen können, die für jeden Frame ausgewertet und aktualisiert werden. Entwickler können auf Eigenschaften auf Kompositionsobjekten oder Eigenschaftensätzen verweisen, mathematische Funktionshilfsbefehle verwenden und sogar auf Eingaben verweisen, um diese mathematischen Beziehungen abzuleiten. Ausdrücke machen Funktionen wie Parallax und „Sticky Headers“ und deren reibungslose Einbindung in die Windows-Plattform möglich.  

##Gründe, die für Kompositionsanimationen sprechen
**Leistung**  
 Bei der Erstellung von universellen Windows-Anwendungen wird der meiste Entwicklercode im UI-Thread ausgeführt. Damit die Animationen flüssig in den verschiedenen Gerätekategorien angezeigt werden, führt das System daher die Animationsberechnungen und-arbeiten auf einem unabhängigen Thread aus, um 60F/s aufrechtzuerhalten. Dies bedeutet, dass sich Entwickler darauf verlassen können, dass das System flüssige Animationen bereitstellt, während ihre Anwendungen andere komplexe Vorgänge für erweiterte Benutzeroberflächen durchführen.    
 
**Möglichkeiten**  
Das Ziel für Kompositionsanimationen in der visuellen Ebene ist es, eine ansprechende Benutzeroberfläche zu ermöglichen. Wir möchten Entwicklern Flexibilität und verschiedene Arten von Animationen bieten, damit sie ihre tollen Ideen umsetzen und die Möglichkeiten von UWP voll ausschöpfen können
 
 (Im [Kompositions-GitHub](http://go.microsoft.com/fwlink/?LinkID=789439) finden Sie Beispiele für die Verwendung der APIs und einige genauere Beispiele der APIs in Aktion)  

**Anwenden von Vorlagen**  
 Alle Kompositionsanimationen in der visuellen Ebene sind Vorlagen – dies bedeutet, dass Entwickler eine Animation auf mehreren Objekten verwenden können, ohne separate Animationen erstellen zu müssen. Dadurch können Entwickler dieselben Animations- und Optimierungseigenschaften oder -Parameter verwenden, um andere Aufgaben durchzuführen, ohne sich Gedanken zu machen, die vorherigen Animationen zu überschreiben.  
 
##Was können Sie mit Kompositionsanimationen animieren?
Kompositionsanimationen können auf die meisten Eigenschaften von Kompositionsobjekten wie visuellen und InsetClip-Objekten angewendet werden. Sie können Kompositionsanimationen auch auf Kompositionseffekte und Eigenschaftensätze anwenden. **Berücksichtigen Sie bei der Auswahl des animierten Objekts den Typ – verwenden Sie diesen, um zu bestimmen, welche Art von Keyframeanimation Sie erstellen, oder in welchen Typ Ihr Ausdruck aufgelöst werden muss.**  
 
###Visuelles Element
|Visuelle Eigenschaften, die animiert werden können|  Typ|
|------|------|
|AnchorPoint|   Vector2|
|CenterPoint|   Vector3|
|Offset|    Vector3|
|Opacity|   Scalar|
|Orientation|   Quaternion|
|RotationAngle| Scalar|
|RotationAngleInDegrees|    Scalar|
|RotationAxis|  Vector3|
|Scale| Vector3|
|Size|  Vector2|
|TransformMatrix*|  Matrix4x4|
*Wenn Sie die gesamte TransformMatrix-Eigenschaft als eine Matrix4x4 animieren möchten, müssen Sie dazu eine Ausdrucksanimation verwenden. Andernfalls können Sie einzelne Zellen der Matrix anvisieren und dort Keyframe- oder Ausdrucksanimationen verwenden.  

###InsetClip
|InsetClip-Eigenschaften, die animiert werden können|   Typ|
|-------------------------------|-------|
|BottomInset|   Scalar|
|LeftInset| Scalar|
|RightInset|    Scalar|
|TopInset|  Scalar|

##Subkanaleigenschaften visueller Elemente
Sie können nicht nur Eigenschaften von visuellen Elementen animieren, sondern auch auf die *Subkanal*-Komponenten dieser Eigenschaften für Animationen abzielen. Angenommen, Sie möchten beispielsweise den X-Offset eines visuellen Elements und nicht den gesamten Offset animieren. Die Animation kann entweder auf die Vector3-Offset-Eigenschaft oder die skalare X-Komponente der Offset-Eigenschaft abzielen. Sie können nicht nur auf eine einzelne Subkanal-Komponente einer Eigenschaft abzielen, sondern auch auf mehrere Komponenten. So können Sie beispielsweise auf die X- und Y-Komponente von Scale abzielen.

|Animierbare Subkanaleigenschaften visueller Elemente|  Typ|
|----------------------------------------|------|
|AnchorPoint.x, y|Scalar|
|AnchorPoint.xy|Vector2|
|CenterPoint.x, y, z|Scalar|
|CenterPoint.xy, xz, yz|Vector2|
|Offset.x, y, z|Scalar|
|Offset.xy, xz, yz|Vector2|
|RotationAxis.x, y, z|Scalar|
|RotationAxis.xy, xz, yz|Vector2|
|Scale.x, y, z|Scalar|
|Scale.xy, xz, yz|Vector2|
|Size.x, y|Scalar|
|Size.xy|Vector2|
|TransformMatrix._11 ... TransformMatrix._NN,|Scalar|
|TransformMatrix._11_12 ... TransformMatrix._NN_NN|Vector2|
|TransformMatrix._11_12_13 ... TransformMatrix._NN_NN_NN|Vector3|
|TransformMatrix._11_12_13_14|Vector4|
|Color*|    Colors (Windows.UI)|

*Das Animieren des Subkanals „Color“ der Brush-Eigenschaft geht ein bisschen anders. Sie fügen StartAnimation() an die Visual.Brush an und deklarieren die zu animierende Eigenschaft im Parameter als „Color“. (Ausführlichere Informationen zum Animieren von Farbe finden Sie weiter unten)

##Eigenschaftensätze und Effekte
Neben der Animation von Eigenschaften von visuellen Kompositions- und InsetClip-Objekten können Sie auch Eigenschaften in einem Eigenschaftensatz oder einem Effekt animieren. Bei Eigenschaftensätzen definieren Sie eine Eigenschaft und speichern Sie in einem Kompositionseigenschaftensatz – diese Eigenschaft kann später das Ziel einer Animation sein (und gleichzeitig kann in einer anderen Animation darauf verwiesen werden). Dies wird in den folgenden Abschnitten ausführlicher erläutert.  

Bei Effekten können Sie mithilfe der Kompositionseffekte-APIs Grafikeffekte definieren (hier finden Sie den [Überblick über Effekte](./composition-effects.md). Zusätzlich zur Definition von Effekten können Sie auch die Eigenschaftenwerte des Effekts animieren. Dazu erfolgt eine Ausrichtung auf die Eigenschaftenkomponente der Brush-Eigenschaft auf visuellen Sprite-Elementen.

##Schnelle Formel: Erste Schritte mit Kompositionsanimationen
Bevor wir tiefer in die Erstellung und Verwendung der unterschiedlichen Arten von Animationen einsteigen, finden Sie unten eine schnelle, allgemeine Formel dafür, wie Sie Kompositionsanimationen zusammenstellen.  
1.  Entscheiden Sie, welche Eigenschaft, Subkanaleigenschaft oder welchen Effekt Sie animieren möchten – notieren Sie den Typ.  
2.  Erstellen Sie ein neues Objekt für die Animation – dies ist entweder eine Keyframe- oder eine Ausdrucksanimation.  
    *  Stellen Sie bei Keyframeanimationen sicher, dass Sie einen Keyframeanimationstyp erstellen, der dem Typ der Eigenschaft entspricht, die Sie animieren möchten.  
    *  Es gibt nur einen einzigen Typ von Ausdrucksanimation.  
3.  Definieren Sie den Inhalt für die Animation – fügen Sie Ihre Keyframes ein, oder definieren Sie die Ausdruckszeichenfolge  
    *  Achten Sie bei Keyframeanimationen darauf, dass der Wert Ihrer Keyframes den gleichen Typ wie die Eigenschaft aufweist, die Sie animieren möchten.  
    *  Stellen Sie bei Ausdrucksanimationen sicher, dass Ihre Ausdruckszeichenfolge in den gleichen Typ wie die Eigenschaft aufgelöst wird, die Sie animieren möchten.  
4.  Starten Sie die Animation auf dem visuellen Objekt, dessen Eigenschaft zu animieren möchten – rufen Sie StartAnimation auf, und schließen Sie folgende Parameter ein: den Namen der Eigenschaft (in Form einer Zeichenfolge), die animiert werden soll, und das Objekt für die Animation.  

```cs
// KeyFrame Animation Example to target Opacity property
// Step 2 - Create your animation object
var animation = _compositor.CreateScalarKeyFrameAnimation();
// Step 3 - Define Content
animation.InsertKeyFrameAnimation(1.0f, 0.2f); 
// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation("Opacity", animation); 
  
// Expression Animation Example to target Opacity property
// Step 2 - Create your animation object
var expression = _compositor.CreateExpressionAnimation(); 
// Step 3 - Define Content (you can also define the string as part of the expression object
// declaration)
expression.Expression = "targetVisual.Offset.X / windowWidth";
expression.SetReferenceParameter("targetVisual", _target);
expression.SetScalarParameter("windowWidth", _xSizeWindow);
// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation("Opacity", expression);

```

##Verwenden von Keyframeanimationen
Keyframeanimationen sind zeitbasierte Animationen, die einen oder mehrere Keyframes verwenden, um anzugeben, wie der animierte Wert im Laufe der Zeit geändert werden muss. Die Frames stellen Markierungen oder Kontrollpunkte dar, mit denen Sie den animierten Wert zu einem bestimmten Zeitpunkt festlegen können.  
 
###Erstellen von Animationen und Definieren von Keyframes
Verwenden Sie zum Erstellen einer Keyframeanimation die Konstruktormethode Ihres Kompositorobjekts, das dem Strukturtyp der zu animierenden Eigenschaft entspricht. Die verschiedenen Typen von Keyframeanimation sind:
*   ColorKeyFrameAnimation
*   QuaternionKeyFrameAnimation
*   ScalarKeyFrameAnimation
*   Vector2KeyFrameAnimation
*   Vector3KeyFrameAnimation
*   Vector4KeyFrameAnimation  

Ein Beispiel, bei dem eine Vector3-Keyframeanimation erstellt wird:     
```cs
var animation = _compositor.CreateVector3KeyFrameAnimation(); 
```

Jede Keyframeanimation wird erstellt, indem einzelne Keyframesegmente eingefügt werden, die zwei Komponenten (mit einer optionalen dritten) definieren  
*   Zeit: normalisierter Fortschrittszustand des Keyframes zwischen 0,0 und 1,0
*   Wert: bestimmter Wert des Animationswerts im Zeitstatus
*   (Optional) Beschleunigungsfunktion: Funktion zum Beschreiben der Interpolation zwischen vorherigem und aktuellem Keyframe (wird später in diesem Thema behandelt)  

Ein Beispiel, bei dem ein Keyframe in der Mitte der Animation eingefügt wird:
```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```


            **Hinweis:** Beim Animieren von Farbe mit Keyframeanimationen müssen ein paar weitere Dinge beachtet werden:
1.  Sie fügen StartAnimation an die Visual.Brush an, anstatt an das Visual-Objekt, wobei **Farbe** als der Eigenschaftsparameter verwendet wird, den Sie animieren möchten.
2.  Die Komponente „Value“ des Keyframes wird durch das Farben-Objekt aus dem Windows.UI-Namespace definiert.
3.  Sie haben die Möglichkeit, das Farbspektrum zu definieren, das die Interpolation durchläuft, indem Sie die Eigenschaft „InterpolationColorSpace“ festlegen. Mögliche Werte umfassen: a.  CompositionColorSpace.Rgb b.  CompositionColorSpace.Hsl


##Eigenschaften von Keyframeanimationen
Nachdem Sie die Keyframeanimation und die einzelnen Keyframes definiert haben, können Sie mehrere Eigenschaften der Animation definieren:
*   Verzögerungszeit – Zeit vor dem Starten einer Animation, nachdem „StartAnimation()“ aufgerufen wurde
*   Dauer – Dauer der Animation
*   Iterationsverhalten – Anzahl bzw. unendliches Wiederholungsverhalten einer Animation
*   Iterationsanzahl – Anzahl der finiten Wiederholungen einer Keyframeanimation
*   Keyframeanzahl – Anzahl von Keyframes in einer bestimmten Keyframeanimation
*   Stoppverhalten – legt das Verhalten eines Animationseigenschaftswerts fest, wenn „StopAnimation“ aufgerufen wird.  

Ein Beispiel, das die Dauer der Animation auf 5 Sekunden festlegt:  
```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

##Beschleunigungsfunktionen
Beschleunigungsfunktionen (CompositionEasingFunction) geben an, wie Zwischenwerte vom vorherigen Keyframewert zum aktuellen Keyframewert voranschreiten. Wenn Sie keine Beschleunigungsfunktion für den Keyframe angeben, wird eine Standardkurve verwendet.  
Es werden zwei Arten von Beschleunigungsfunktionen unterstützt:
*   Linear
*   Kubische Bézierkurve  

Die kubische Bézierkurve ist eine parametrische Funktion, die häufig verwendet wird, um gleichmäßige Kurven zu beschreiben, die skaliert werden können. Bei Verwendung mit Kompositions-Keyframeanimationen definieren Sie zwei Kontrollpunkte, die Vector2-Objekte sind. Diese Kontrollpunkte werden verwendet, um die Form der Kurve zu definieren. Es wird empfohlen, ähnliche Websites wie [diese](http://cubic-bezier.com/#0,-0.01,.48,.99) zu verwenden, um zu veranschaulichen, wie die beiden Kontrollpunkte die Kurve für eine kubische Bézierkurve erstellen.

Um eine Beschleunigungsfunktion zu erstellen, nutzen Sie die Konstruktormethode aus Ihrem Kompositorobjekt. Unten finden Sie zwei Beispiele, die eine lineare Beschleunigungsfunktion und eine einfache kubische Bézierkurve erstellen.  
```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```
Um Ihre Beschleunigungsfunktion zu Ihrem Keyframe hinzuzufügen, fügen Sie einfach den dritten Parameter zum Keyframe hinzu, wenn sie ihn in die Animation einfügen:   
Ein Beispiel, das eine easeIn-Beschleunigungsfunktion mit dem Keyframe hinzufügt:  
```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

##Starten und Beenden von Keyframeanimationen
Nachdem Sie Ihre Animation und Keyframes definiert haben, können Sie Ihre Animation einbinden. Wenn Sie die Animation starten, geben Sie das visuelle Objekt an, das animiert werden soll, die Zieleigenschaft, die animiert werden soll, sowie einen Verweis auf die Animation. Dazu rufen Sie die StartAnimation()-Funktion auf. Bedenken Sie, dass durch das Aufrufen von StartAnimation() für eine Eigenschaft alle zuvor ausgeführten Animationen getrennt und entfernt werden.  

            **Hinweis:** Der Verweis auf die Eigenschaft, die Sie animieren möchten, weist die Form einer Zeichenfolge auf.  

Ein Beispiel, bei dem eine Animation für die Offset-Eigenschaft eines visuellen Objekts festgelegt und gestartet wird:  
```cs
targetVisual.StartAnimation("Offset", animation);
```  

Wenn Sie auf eine Unterkanaleigenschaft abzielen möchten, fügen Sie den Unterkanal in die Zeichenfolge ein, welche die Eigenschaft definiert, die Sie animieren möchten. In den obigen Beispielen würde die Syntax in StartAnimation("Offset.X, animation2) geändert werden, wobei animation2 eine ScalarKeyFrameAnimation ist.  

Nach dem Starten Ihrer Animation haben Sie auch die Möglichkeit, sie anzuhalten, bevor sie beendet wird. Dazu wird die StopAnimation()-Funktion verwendet.  
Ein Beispiel, bei dem eine Animation auf der Offset-Eigenschaft eines visuellen Objekts angehalten wird:    
```cs
targetVisual.StopAnimation("Offset");
```

Sie haben auch die Möglichkeit, das Verhalten der Animation zu definieren, wenn sie explizit beendet wird. Dazu definieren Sie die Eigenschaft für das Stoppverhalten für Ihre Animation. Es gibt drei Möglichkeiten:
*   LeaveCurrentValue: Die Animation markiert den Wert der animierten Eigenschaft als den letzten berechneten Wert der Animation
*   SetToFinalValue: Die Animation markiert den Wert der animierten Eigenschaft als den Wert des letzten Keyframes
*   SetToFinalValue: Die Animation markiert den Wert der animierten Eigenschaft als den Wert des ersten Keyframes  

Ein Beispiel, bei dem die StopBehavior-Eigenschaft für eine Keyframeanimation festgelegt wird:  
```cs
animation.StopBehavior = AnimationStopBehavior.LeaveCurrentValue;
```

##Abschlussereignisse für Animationen
Bei Keyframeanimationen können Entwickler Animationsstapel verwenden, die aggregiert werden, wenn eine bestimmte Animation (oder Gruppe von Animationen) abgeschlossen wurde. Es können nur Abschlussereignisse für Keyframeanimationen zusammengefasst werden. Ausdrücke haben kein definitives Ende, sodass kein Abschlussereignis ausgelöst wird. Wenn eine Ausdrucksanimation in einem Stapel gestartet wird, wird die Animation wie erwartet ausgeführt und davon nicht betroffen, wenn der Stapel ausgelöst wird.    

Ein Stapelabschlussereignis wird ausgelöst, wenn alle Animationen im Stapel abgeschlossen sind. Die Zeitspanne bis zum Auslösen eines Stapelereignisses richtet sich nach der längsten oder der am meisten verzögerten Animation im Stapel.
Das Aggregieren von Endzuständen ist hilfreich, wenn Sie wissen müssen, wann Gruppen bestimmter Animationen abgeschlossen sind, um andere Aufgaben zu planen.  

Stapel werden gelöscht, sobald das Abschlussereignis ausgelöst wurde. Sie können Dispose() auch jederzeit aufrufen, um die Ressource frühzeitig freizugeben. Sie sollten das Batch-Objekt manuell freigeben, wenn eine Animation im Batch frühzeitig beendet wird und das Abschlussereignis nicht gestartet werden soll. Wenn eine Animation unterbrochen oder abgebrochen wird, wird das Abschlussereignis ausgelöst und beim Stapel mitgezählt, in dem es festgelegt wurde. Dies wird im Animation_Batch-SDK-Beispiel auf dem [Windows/Kompositions-GitHub](http://go.microsoft.com/fwlink/p/?LinkId=789439) veranschaulicht.  
 
##Bewertete Stapel
Um eine bestimmte Gruppe von Animationen zu aggregieren oder auf das Abschlussereignis einer einzelnen Animation abzuzielen, erstellen Sie einen bewerteten Stapel.    
```cs
CompositionScopedBatch myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
``` 
Nach dem Erstellen eines bewerteten Stapels werden alle gestarteten Animationen aggregiert, bis der Stapel mit der Funktion „Suspend“ oder „End“ explizit angehalten oder beendet wird.    

Durch Aufrufen der Suspend-Funktion wird die Aggregation von Animationen angehalten, bis „Resume“ aufgerufen wird. So können Sie Inhalte aus einem bestimmten Stapel explizit ausschließen.  

Im folgenden Beispiel wird die Animation, die auf die Offset-Eigenschaft von VisualA abzielt, nicht in den Stapel eingeschlossen:  
```cs
myScopedBatch.Suspend();
VisualA.StartAnimation("Offset", myAnimation);
myScopeBatch.Resume();
```

Zum Beenden des Stapels müssen Sie End() aufrufen. Wenn „End“ nicht aufgerufen wird, bleibt der Stapel geöffnet und erfasst weiterhin Objekte.  
 
Der folgende Codeausschnitt und die folgende Abbildung zeigen ein Beispiel dafür, wie der Batch Animationen zum Nachverfolgen von Endzuständen aggregiert. Beachten Sie, dass die Animationen 1, 3 und 4 – jedoch nicht 2 – in diesem Beispiel Endzustände aufweisen, die durch diesen Stapel nachverfolgt werden.  
```cs
myScopedBatch.End();
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
// Start Animation1
[…]
myScopedBatch.Suspend();
// Start Animation2 
[…]
myScopedBatch.Resume();
// Start Animation3
[…]
// Start Animation4
[…]
myScopedBatch.End();
```  
![](./images/composition-scopedbatch.png)
 
##Batchverarbeitung des Abschlussereignisses einer einzelnen Animation
Wenn Sie möchten wissen, wann eine einzelne Animation endet, müssen Sie einen bewerteten Stapel erstellen, der nur die entsprechende Animation enthält. Beispiel:  
```cs
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

##Abrufen des Abschlussereignisses eines Stapel

Bei der Batchverarbeitung einer oder mehrerer Animationen rufen Sie das Abschlussereignis des Stapels genauso ab. Sie registrieren die Ereignisbehandlungsmethode für das „Completed“-Ereignis des entsprechenden Stapels.  

```cs
myScopedBatch.Completed += OnBatchCompleted;
``` 

##Stapelzustände
Es gibt zwei Eigenschaften, die Sie verwenden können, um den Zustand eines vorhandenen Stapels zu bestimmen. IsActive und IsEnded.  

Die IsActive-Eigenschaft gibt „true“ zurück, wenn ein Stapel zum Aggregieren von Animationen geöffnet ist. IsActive gibt „false“ zurück, wenn ein Batch angehalten oder beendet wurde.   

Die IsEnded-Eigenschaft gibt „true“ zurück, wenn Sie eine Animation diesem spezifischen Stapel nicht hinzufügen können. Ein Batch wird beendet, wenn Sie für einen bestimmten Batch explizit „End()“ aufrufen.  
 
##Verwenden von Ausdrucksanimationen
Ausdrucksanimationen sind eine neue Art von Animation, die vom Kompositionsteam mit dem November-Update für Windows 10 (10586) eingeführt wurde. Allgemein gesagt, basieren Ausdrucksanimationen auf einer mathematischen Formel/Beziehung zwischen den einzelnen Werten und Verweisen auf andere Kompositionsobjekteigenschaften. Im Gegensatz zu Keyframeanimationen, die eine Interpolatorfunktion (kubische Bézierkurve, Quad, Quintic usw.) verwenden, um zu beschrieben, wie sich der Wert im Laufe der Zeit ändert, verwenden Ausdrucksanimationen eine mathematische Formel, um zu definieren, wie der animierte Wert für jeden Frame berechnet wird. Es muss darauf hingewiesen werden, dass Ausdrucksanimationen keine definierte Dauer haben – nach dem Start werden sie so lange ausgeführt und verwenden die mathematische Formel zur Ermittlung des Werts der Animationseigenschaft, bis sie explizit beendet werden.

**Warum sind Ausdrucksanimationen hilfreich?** Der wahre Vorteil von Ausdrucksanimationen liegt in deren Fähigkeit, eine mathematische Beziehung zu erstellen, die Verweise auf Parameter oder Eigenschaften auf anderen Objekten enthält. Dies bedeutet, dass Sie eine Formel haben können, die auf Werte von Eigenschaften auf anderen Kompositionsobjekten, lokale Variablen oder auch gemeinsam genutzte Werte in Kompositionseigenschaftensätzen verweist. Aufgrund dieses Referenzmodells und der Tatsache, dass die Formel für jeden Frame ausgewertet wird, ändert sich die Ausgabe der Formel, wenn sich die Werte ändern, die eine Formel definieren. Dies eröffnet mehr Möglichkeiten, die über herkömmliche Keyframeanimationen hinausgehen, bei denen Werte separat und vordefiniert sein müssen. So können z. B. Funktionen wie Sticky Headers und Parallax problemlos mit Ausdrucksanimationen beschrieben werden.


            **Hinweis:** Wir verwenden die Begriffe „Ausdruck“ oder „Ausdruckszeichenfolge“ im Bezug auf Ihre mathematische Formel, die Ihr Ausdrucksanimationsobjekt definiert.

##Erstellen und Anfügen Ihrer Ausdrucksanimation
Bevor wir näher auf die Syntax für das Erstellen von Ausdrucksanimationen eingeben, müssen einige wesentliche Prinzipien erwähnt werden:  
*   Ausdrucksanimationen verwenden eine definierte mathematische Formel, um den Wert der animierten Eigenschaft für jeden Frame zu bestimmen.
*   Die mathematische Formel wird in den Ausdruck als Zeichenfolge eingegeben.
*   Die Ausgabe der mathematischen Formel muss in den gleichen Typ wie die Eigenschaft aufgelöst werden, die Sie animieren möchten. Wenn sie nicht übereinstimmen, erhalten Sie bei der Berechnung des Ausdrucks einen Fehler. Wenn die Formel in Nan (Anzahl/0) aufgelöst wird, verwendet das System den letzten zuvor berechneten Wert.
*   Ausdrucksanimationen haben eine *unendliche Lebensdauer* – sie werden so lange ausgeführt, bis sie beendet werden.  

Zum Erstellen Ihrer Ausdrucksanimation verwenden Sie einfach den Konstruktor aus Ihrem Kompositionsobjekt, in dem Sie Ihren mathematischen Ausdruck definieren.  
 
Ein Beispiel für den Konstruktor, wobei ein sehr einfacher Ausdruck definiert wird, der zwei skalare Werte summiert (im nächsten Abschnitt werden komplexere Ausdrücke behandelt):  
```cs
var expression = _compositor.CreateExpressionAnimation("0.2 + 0.3");
```
Ähnlich wie bei Keyframeanimationen müssen Sie, nachdem Sie Ihre Ausdrucksanimation definiert haben, diese an das visuelle Objekt anhängen und die Eigenschaft deklarieren, welche die Animation animieren soll. Unten fahren wir mit dem obigen Beispiel fort und hängen unsere Ausdrucksanimation an die Opacity-Eigenschaft (ein skalarer Typ) des visuellen Objekts an:  
```cs
targetVisual.StartAnimation("Opacity", expression);
```

##Komponenten Ihrer Ausdruckszeichenfolge
Das Beispiel im vorherigen Abschnitt veranschaulichte zwei einfache skalare Werte, die addiert werden. Obwohl dies ein gültiges Beispiel für Ausdrücke ist, veranschaulicht es nicht das ganze Spektrum an Vorteilen, die Ihnen Ausdrücke bieten. Beim obigen Beispiel ist zu beachten, dass die Gleichung für jeden Frame in 0,5 aufgelöst wird und sich während der gesamten Lebensdauer der Animation niemals ändert, da es sich um separate Werte handelt. Das wirkliche Potenzial von Ausdrücken ergibt sich aus der Definition einer mathematischen Beziehung, in der sich die Werte in regelmäßigen Abständen oder ständig ändern können.  
 
Betrachten wir nun die verschiedenen Teile, aus denen diese Art von Ausdrücken bestehen kann.  

###Operatoren, Rangfolge und Orientierung
Die Ausdruckszeichenfolge unterstützt die Verwendung normaler Operatoren, von denen Sie erwarten würden, dass sie mathematische Beziehungen zwischen unterschiedlichen Komponenten der Gleichung beschreiben:  

|Kategorie|  Operatoren|
|--------|-----------|
|Unär| -|
|Multiplikativ|    * /|
|Additiv|  + -|

Wenn der Ausdruck bewertet wird, hält er auf ähnliche Weise bei der Auswertung des Ausdrucks die Operatorrangfolge und -assoziativität ein, wie in der Spezifikation der C#-Sprache definiert. Anders ausgedrückt, er hält die grundlegende Reihenfolge der Vorgänge ein.  

Im folgenden Beispiel werden – basierend auf der Reihenfolge der Vorgänge – bei einer Auswertung die Klammern zuerst aufgelöst, bevor der Rest der Gleichung aufgelöst wird:  
```cs
"(5.0 * (72.4 – 36.0) + 5.0" // (5.0 * 36.4 + 5) -> (182 + 5) -> 187
```

###Eigenschaftenparameter
Eigenschaftenparameter zählen zu den nützlichsten Komponenten von Ausdrucksanimationen. In der Ausdruckszeichenfolge können Sie auf Werte von Eigenschaften von anderen Objekten, wie z. B. visuellen Kompositionsobjekten, Kompositionseigenschaftssätzen oder anderen C#-Objekten verweisen.   

Um diese in einer Ausdruckszeichenfolge zu verwenden, müssen Sie lediglich die Verweise als Parameter für die Ausdrucksanimation definieren. Dies erfolgt durch die Zuordnung der im Ausdruck verwendeten Zeichenfolge zum eigentlichen Objekt. Dadurch weiß das System beim Auswerten der Gleichung, was überprüft werden muss, um den Wert zu berechnen. Es gibt verschiedene Arten von Parametern, die dem Typ des Objekts entsprechen, das Sie in die Gleichung aufnehmen möchten:  

|Typ|  Funktion zum Erstellen des Parameters|
|----|------------------------------|
|Scalar|    SetScalarParameter(Zeichenfolgenreferenz, Skalarobjekt)|
|Vector|    SetVector2Parameter(Zeichenfolgenreferenz, Vector2-Objekt)<br/>SetVector3Parameter(Zeichenfolgenreferenz, Vector3-Objekt)<br/>SetVector4Parameter(Zeichenfolgenreferenz, Vector4-Objekt)|
|Matrix|    SetMatrix3x2Parameter(Zeichenfolgenreferenz, Matrix3x2-Objekt)<br/>SetMatrix4x4Parameter(Zeichenfolgenreferenz, Matrix4x4-Objekt)|
|Quaternion|    SetQuaternionParameter(Zeichenfolgenreferenz, Quaternion-Objekt)|
|Color| SetColorParameter(Zeichenfolgenreferenz, Color-Objekt)|
|CompositionObject| SetReferenceParameter(Zeichenfolgenreferenz, Kompositionsobjekt)|

Im folgenden Beispiel erstellen wir eine Ausdrucksanimation, die auf den Offset von zwei anderen visuellen Kompositionsobjekten und einem einfachen System.Numerics-Vektor3-Objekt verweist.  
```cs
var commonOffset = new Vector3(25.0, 17.0, 10.0);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + additionalOffset);
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetVector3Parameter("additionalOffset", commonOffset);
```

Darüber hinaus können Sie auf einen Wert in einem Eigenschaftensatz aus einem Ausdruck mit dem oben beschriebenen Modell verweisen. Kompositionseigenschaftensätze stellen eine praktische Möglichkeit zum Speichern von Daten dar, die von Animationen verwendet werden, und eignen sich zur Erstellung wiederverwendbarer Daten, die nicht an die Lebensdauer der anderen Kompositionsobjekte gebunden sind. Auf Eigenschaftensatzwerte kann in einem Ausdruck wie auf andere Eigenschaften verwiesen werden. (Eigenschaftensätze werden in einem späteren Abschnitt ausführlicher erläutert.)  

Wir können das Beispiel direkt oben ändern, indem wir anstelle einer lokalen Variable einen Eigenschaftensatz zum Definieren des CommonOffset verwenden:
```cs
_sharedProperties = _compositor.CreatePropertySet();
_sharedProperties.InsertVector3("commonOffset", offset);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + sharedProperties.commonOffset);
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

Beim Verweisen auf Eigenschaften von anderen Objekten ist es auch möglich, auf die Subkanaleigenschaften in der Ausdruckszeichenfolge oder als Teil des Referenzparameters zu verweisen.  
 
Im folgenden Beispiel verweisen wir auf x Subkanal der Offset-Eigenschaften von zwei visuellen Objekten – eines in der Ausdruckszeichenfolge selbst und das andere beim Erstellen des Parameterverweises.
Beachten Sie, dass wir beim Verweisen auf die X-Komponente von Offset unseren Parametertyp in einen skalaren Parameter anstelle eines Vector3 wie im vorherigen Beispiel ändern:  
```cs
var expression = _compositor.CreateExpressionAnimation("xOffset/ ParentOffset.X");
expression.SetScalarParameter("xOffset", childVisual.Offset.X);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
```

###Hilfsfunktionen und Konstruktoren für Ausdrücke
Zusätzlich zum Zugriff auf Operatoren und Eigenschaftenparameter können Sie eine Liste mathematischer Funktionen in den Ausdrücken verwenden. Diese Funktionen werden zur Durchführung von Berechnungen und Vorgängen für unterschiedliche Typen durchgeführt, die Sie auch mit System.Numerics-Objekten durchführen würden.  

Im Beispiel unten wird ein Ausdruck für Skalare erstellt, bei dem die Hilfsfunktion „Clamp“ verwendet:  
```cs
var expression = _compositor.CreateExpressionAnimation("Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)"
```

Neben einer Liste mit Hilfsfunktionen können Sie auch integrierte Konstruktormethoden in einer Zeichenfolge verwenden, die eine Instanz dieses Typs auf Grundlage der bereitgestellten Parameter generieren.  

Im Beispiel unten wird ein Ausdruck erstellt, bei dem ein neuer Vector3 in der Ausdruckszeichenfolge definiert wird:  
```cs
var expression = _compositor.CreateExpressionAnimation("Offset / Vector3(targetX, targetY, targetZ");
```

Sie finden die vollständige umfassende Liste mit Hilfsfunktionen und Konstruktoren im Anhang oder für jeden Typ in der folgenden Liste:  
*   [Scalar](#scalar)
*   [Vector2](#vector2)
*   [Vector3](#vector3)
*   [Matrix3x2](#matrix3x2)
*   [Matrix4x4](#matrix4x4)
*   [Quaternion](#quaternion)
*   [Color](#color)  

###Ausdrucksschlüsselwörter
Sie können spezielle „Schlüsselwörter“ verwenden, die anders behandelt werden, wenn die Ausdruckszeichenfolge ausgewertet wird. Weil sie als „Schlüsselwörter“ angesehen werden, können sie nicht als Zeichenfolgeparameterabschnitt ihrer Eigenschaftsverweise verwendet werden.  
 
|Schlüsselwort|   Beschreibung|
|-------|--------------|
|This.StartingValue| Stellt einen Verweis auf den ursprünglichen Anfangswert der Eigenschaft, die animiert wird, bereit.|
|This.CurrentValue| Stellt einen Verweis auf den derzeit „bekannten“ Wert der Eigenschaft bereit|
|Pi| Stellt einen Schlüsselwortverweis auf den Wert von PI bereit|

Ein Beispiel unten, das die Verwendung dieses StartingValue-Schlüsselworts veranschaulicht:  
```cs
var expression = _compositor.CreateExpressionAnimation("this.StartingValue + delta");
```

###Ausdrücke mit Bedingungen
Neben der Unterstützung mathematischer Beziehungen mit Operatoren, Verweisen auf Eigenschaften und Funktionen und Konstruktoren können Sie auch einen Ausdruck erstellen, der einen ternären Operator enthält:  
```
(condition ? ifTrue_expression : ifFalse_expression)
```

Mit bedingten Anweisungen können Sie Ausdrücke so schreiben, dass verschiedene mathematische Beziehungen basierend auf einer bestimmten Bedingung vom System verwendet werden, um den Wert der animierten Eigenschaft zu berechnen. Ternäre Operatoren können als die Ausdrücke für die Anweisungen „true“ oder „false“ geschachtelt werden.  

Die folgenden bedingten Operatoren werden in der Bedingungsanweisung unterstützt: 
*   Ist gleich (==)
*   Ist ungleich (!=)
*   Kleiner als (<)
*   Kleiner oder gleich (<=)
*   Größer als (>)
*   Größer oder gleich (>=)  

Die folgenden Konjunktionen werden als Operatoren oder Funktionen in der Bedingungsanweisung unterstützt:
*   Not: ! / Not(bool1)
*   And: && / And(bool1, bool2)
*   Or: || / Or(bool1, bool2)  

Es folgt ein Beispiel für eine Ausdrucksanimation mit einer Bedingung.  
```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f + (target.Offset.x / parent.Offset.x) : 1.0f");
```

##Ausdruckskeyframes
Weiter oben in diesem Dokument wurde beschrieben, wie Sie Keyframeanimationen erstellen, und es wurden Ausdrucksanimationen und all die verschiedenen Komponenten vorgestellt, die Sie zur Erstellung der Ausdruckszeichenfolge verwenden können. Was, wenn Sie die Vorteile von Ausdrucksanimationen mit der Zeitinterpolation von Keyframeanimationen kombinieren möchten? Die Antwort lautet: Ausdruckskeyframes.  

Anstatt einen separaten Wert für jeden Kontrollpunkt in der Keyframeanimation zu definieren, kann der Wert eine Ausdruckszeichenfolge sein. In diesem Fall verwendet das System die Ausdruckszeichenfolge dafür, um den Wert zu berechnen, den die animierte Eigenschaft an einem bestimmten Punkt auf der Zeitachse haben sollte. Das System interpoliert dann einfach auf diesen Wert, wie bei einer normalen Keyframeanimation.    

Sie müssen keine speziellen Animationen erstellen, um Ausdruckskeyframes zu verwenden – fügen Sie einen Ausdruckskeyframe einfach in Ihre standardmäßige Keyframeanimation ein, geben Sie die Zeit und Ihre Ausdruckszeichenfolge als Wert an. Das folgende Beispiel veranschaulicht dies. Dabei wird eine Ausdruckszeichenfolge als Wert für einen der Keyframes verwendet:   
```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

##Ausdrucksbeispiel
Der folgende Code zeigt ein Beispiel für das Einrichten einer Ausdrucksanimation für eine einfache Parallax-Funktion, die Eingabewerte aus der Bildlaufanzeige abruft.
```cs
// Get scrollviewer
ScrollViewer myScrollViewer = ThumbnailList.GetFirstDescendantOfType<ScrollViewer>();
_scrollProperties = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);

// Setup the expression
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 *  ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5   * ItemHeight))";
```

##Animieren mit Eigenschaftensätzen
Kompositionseigenschaftensätze bieten Ihnen die Möglichkeit zum Speichern von Werten, die für mehrere Animationen freigegeben werden können und nicht an die Lebensdauer eines anderen Kompositionsobjekts gebunden sind. Eigenschaftensätze sind sehr nützlich, um allgemeine Werte zu speichern. Auf diese kann später in Animationen auf einfache Weise verwiesen werden. Sie können Eigenschaftensätze auch verwenden, um Daten auf Grundlage von Anwendungslogik zur Steuerung eines Ausdrucks zu speichern.  

Um einen Eigenschaftensatz zu erstellen, verwenden Sie die Konstruktormethode aus Ihrem Kompositorobjekt:  
```cs
_sharedProperties = _compositor.CreatePropertySet();
```

Nachdem Sie den Eigenschaftensatz erstellt haben, können Sie eine Eigenschaft und einen Wert hinzufügen:  
```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

Ähnlich wie oben können wir auf diesen Eigenschaftensatzwert in einer Ausdrucksanimation verweisen:  
```cs
var expression = _compositor.CreateExpressionAnimation("this.target.Offset + sharedProperties.NewOffset");
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
targetVisual.StartAnimation("Offset", expression);
```

Eigenschaftensatzwerte können auch animiert werden. Dazu wird die Animation an das PropertySet-Objekt angefügt. Auf den Eigenschaftennamen wird dann in der Zeichenfolge verwiesen. Unten animieren wir die NewOffset-Eigenschaft im Eigenschaftensatz mithilfe einer Keyframeanimation.  
```cs
var keyFrameAnimation = _compositor.CreateVector3KeyFrameAnimation()
keyFrameAnimation.InsertKeyFrame(0.5f, new Vector3(25.0f, 68.0f, 0.0f);
keyFrameAnimation.InsertKeyFrame(1.0f, new Vector3(89.0f, 125.0f, 0.0f);
_sharedProperties.StartAnimation("NewOffset", keyFrameAnimation);
```


Sie fragen sich vielleicht, was mit dem Wert der animierten Eigenschaft passiert, an welche die Ausdrucksanimation angehängt ist, wenn dieser Code in einer App ausgeführt wird. In diesem Fall würde der Ausdruck anfänglich einen Wert ausgeben. Sobald die Keyframeanimation allerdings beginnt, die Eigenschaft im Eigenschaftensatz zu animieren, wird auch der Ausdruckswert aktualisiert, da die Formel für jeden Frame berechnet wird. Dies ist der Vorteil von Eigenschaftensätzen mit Ausdrucks- und Keyframeanimationen.  
 
##ManipulationPropertySet
Zusätzlich zur Nutzung von Composition-Eigenschaftensätzen kann ein Entwickler auch auf ManipulationPropertySet und so auf die Eigenschaften eines XAML-ScrollViewer-Elements zugreifen. Diese Eigenschaften können dann verwendet und in einer Ausdrucksanimation referenziert werden. So sind Parallax- und „Sticky Header“-Umgebungen möglich. Hinweis: Sie können das ScrollViewer-Element jedes XAML-Steuerelements (ListView, GridView usw.) verwenden, das bildlauffähige Inhalte hat und das ScrollViewer-Elemente nutzt, um das ManipulationPropertySet-Element für diese bildlauffähigen Steuerelemente zu nutzen.  

In Ihrem Ausdruck können Sie die folgenden Eigenschaften der Bildlaufanzeige nutzen:  

|Eigenschaft| Typ|  
|--------|-----|  
|Translation| Vector3|  
|Pan| Vector3|  
|Scale| Vector3|  
|CenterPoint| Vector3|  
|Matrix| Matrix4x4|  

Verwenden Sie zum Abrufen einer Referenz für das ManipulationPropertySet-Element die GetScrollViewerManipulationPropertySet-Methode von ElementCompositionPreview.  
```csharp
CompositionPropertySet manipPropSet = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScroller);
```

Wenn Sie die Referenz auf den Eigenschaftensatz haben, können Sie Eigenschaften der Bildlaufanzeige in dem Eigenschaftensatz nutzen. Als Erstes erstellen Sie die Referenzparameter.  
```csharp
ExpressionAnimation exp = compositor.CreateExpressionAnimation();
exp.SetReferenceParameter("ScrollManipulation", manipPropSet);
```

Nach dem Einrichten des Referenz-Parameters können Sie die Eigenschaften von ManipulationPropertySet im Ausdruck verwenden.  
```csharp
exp.Expression = “ScrollManipulation.Translation.Y / ScrollBounds”;
_target.StartAnimation(“Opacity”, exp);
```



 
 
##Anhang
###Ausdrucksfunktionen nach Strukturtyp
###Scalar  

|Funktion und Konstruktorvorgänge| Beschreibung|  
|-----------------------------------|--------------|  
|Abs(Float-Wert)|  Gibt einen Float-Wert zurück, der den absoluten Wert des Float-Parameters darstellt|  
|Clamp(Float-Wert, Float Min., Float Max.)|  Gibt einen Float-Wert zurück, der größer als „Min.“ und kleiner als „Max.“ oder „Min.“ ist, wenn der Wert kleiner als „Min.“ oder „Max.“ ist, wenn der Wert größer als „Max.“ ist|  
|Max (Float-Wert1, Float-Wert2)|  Gibt den größeren Float-Wert zwischen Wert1 und Wert2 zurück|  
|Min (Float-Wert1, Float-Wert2)|  Gibt den kleineren Float-Wert zwischen Wert1 und Wert2 zurück|  
|Lerp(Float-Wert1, Float-Wert2, Float-Fortschritt)|  Gibt einen Float-Wert zurück, der die berechnete lineare Interpolation zwischen den beiden skalaren Werten basierend auf dem Fortschritt darstellt (Hinweis: Fortschritt ist zwischen 0,0 und 1,0)|  
|Slerp(Float-Wert1, Float-Wert2, Float-Fortschritt)| Gibt einen Float-Wert zurück, der die berechnete sphärische Interpolation zwischen den beiden Float-Werten basierend auf dem Fortschritt darstellt (Hinweis: Fortschritt ist zwischen 0,0 und 1,0)|  
|Mod(Float-Wert1, Float-Wert2)|   Gibt den Float-Rest aus der Teilung von Wert1 und Wert2 zurück|  
|Ceil(Float-Wert)|     Gibt den auf die nächste größere Ganzzahl gerundeten Float-Parameter zurück|  
|Floor(Float-Wert)|    Gibt den auf die nächste kleinere Ganzzahl gerundeten Float-Parameter zurück|  
|Sqrt(Float-Wert)| Gibt die Quadratwurzel des Float-Parameters zurück|  
|Square(Float-Wert)|   Gibt das Quadrat des Float-Parameters zurück|  
|Sin(Float-Wert1)||
|Asin(Float-Wert2)|    Gibt den Sin oder ArcSin des Float-Parameters zurück|
|Cos(Float-Wert1)||
|ACos(Float-Wert2)|    Gibt den Cos oder ArcCos des Float-Parameters zurück|
|Tan(Float-Wert1)||
|ATan(Float-Wert2)|    Gibt den Tan oder ArcTan des Float-Parameters zurück|
|Round(Float-Wert)|    Gibt den auf die nächste Ganzzahl gerundeten Float-Parameter zurück|
|Log10(Float-Wert)|    Gibt das Protokollergebnis (Basis 10) des Float-Parameters zurück|
|Ln(Float-Wert)|   Gibt das natürliche Protokollergebnis des Float-Parameters zurück|
|Pow(Float-Wert, Float-Potenz)| Gibt das Ergebnis des auf eine bestimmte Potenz erhobenen Float-Parameters zurück|
|ToDegrees(Float-Bogenmaß)|  Gibt den in Grad konvertierten Float-Parameter zurück|
|ToRadians(Float-Grad)|  Gibt den in Bogenmaß konvertierten Float-Parameter zurück|

###Vector2  

|Funktion und Konstruktorvorgänge|   Beschreibung|
|-----------------------------------|--------------|
|Abs (Vector2-Wert)|   Gibt einen Vector2 mit einem auf jede Komponente angewendeten absoluten Wert zurück|
|Clamp (Vector2-Wert1, Vector2 Min., Vector2 Max.)|  Gibt einen Vector2 zurück, der die arretierten Werte für die jeweilige Komponente enthält|
|Max (Vector2-Wert1, Vector2-Wert2)|  Gibt einen Vector2 zurück, der für jede entsprechende Komponente von Wert1 und Wert2 ein Max. durchgeführt hat|
|Min (Vector2-Wert1, Vector2-Wert2)|  Gibt einen Vector2 zurück, die für jede entsprechende Komponente von Wert1 und Wert2 ein Min. durchgeführt hat|
|Scale(Vector2-Wert, Float-Faktor)|    Gibt einen Vector2 zurück, wobei jede Komponente des Vektors mit dem Skalierungsfaktor multipliziert wurde.|
|Transform(Vector2-Wert, Matrix3x2-Matrix)|    Gibt einen Vector2 zurück, der sich aus der linearen Transformation zwischen einem Vector2 und einer Matrix3x2 ergibt (auch bekannt als Multiplizieren eines Vektors mit einer Matrix).|
|Lerp(Vector2-Wert1, Vector2-Wert2, Float-Fortschritt)|  Gibt einen Vector2 zurück, der die berechnete lineare Interpolation zwischen den beiden Vector2-Werten basierend auf dem Fortschritt darstellt (Hinweis: Fortschritt ist zwischen 0,0 und 1,0)|
|Length(Vector2-Wert)| Gibt einen Float-Wert zurück, der die Länge/Größe von Vector2 darstellt|
|LengthSquared(Vector2)|    Gibt einen Float-Wert zurück, der das Quadrat der Länge/Größe eines Vector2 darstellt|
|Distance(Vector2-Wert1, Vector2-Wert2)|  Gibt einen Float-Wert zurück, der den Abstand zwischen zwei Vector2-Werten darstellt|
|DistanceSquared(Vector2-Wert1, Wert2-Vector2)|   Gibt einen Float-Wert zurück, der das Quadrat zwischen zwei Vector2-Werten darstellt|
|Normalize(Vector2-Wert)|  Gibt einen Vector2-Wert zurück, der den Einheitsvektor des Parameters darstellt, wobei alle Komponenten normalisiert wurden|
|Vector2(Float x, Float y)| Erstellt einen Vector2 mit zwei Float-Parametern|

###Vector3  

|Funktion und Konstruktorvorgänge|   Beschreibung|
|-----------------------------------|--------------|
|Abs (Vector3-Wert)|   Gibt einen Vector3 mit einem auf jede Komponente angewendeten absoluten Wert zurück|
|Clamp (Vector3-Wert1, Vector3 Min., Vector3 Max.)|  Gibt einen Vector3 zurück, der die arretierten Werte für die jeweilige Komponente enthält|
|Max (Vector3-Wert1, Vector3-Wert2)|  Gibt einen Vector3 zurück, die für jede entsprechende Komponente von Wert1 und Wert2 ein Max. durchgeführt hat|
|Min (Vector3-Wert1, Vector3-Wert2)|  Gibt einen Vector3 zurück, die für jede entsprechende Komponente von Wert1 und Wert2 ein Min. durchgeführt hat|
|Scale(Vector3-Wert, Float-Faktor)|    Gibt einen Vector3 zurück, wobei jede Komponente des Vektors mit dem Skalierungsfaktor multipliziert wurde.|
|Lerp(Vector3-Wert1, Vector3-Wert2, Float-Fortschritt)|  Gibt einen Vector3 zurück, der die berechnete lineare Interpolation zwischen den beiden Vector3-Werten basierend auf dem Fortschritt darstellt (Hinweis: Fortschritt ist zwischen 0,0 und 1,0)|
|Länge(Vector3-Wert)| Gibt einen Float-Wert zurück, der die Länge/Größe von Vector3 darstellt|
|LengthSquared(Vector3)|    Gibt einen Float-Wert zurück, der das Quadrat der Länge/Größe eines Vector3 darstellt|
|Distance(Vector3-Wert1, Vector3-Wert2)|  Gibt einen Float-Wert zurück, der den Abstand zwischen zwei Vector3-Werten darstellt|
|DistanceSquared(Vector3-Wert1, Vector3-Wert2)|   Gibt einen Float-Wert zurück, der das Quadrat zwischen zwei Vector3-Werten darstellt|
|Normalize(Vector3-Wert)|  Gibt einen Vector3-Wert zurück, der den Einheitsvektor des Parameters darstellt, wobei alle Komponenten normalisiert wurden|
|Vector3(Float x, Float y, Float z)|    Erstellt einen Vector3 mit drei Float-Parametern|

###Vector4  

|Funktion und Konstruktorvorgänge|   Beschreibung|
|-----------------------------------|--------------|
|Abs (Vector4-Wert)|   Gibt einen Vector3 mit einem auf jede Komponente angewendeten absoluten Wert zurück|
|Clamp (Vector4-Wert1, Vector4 Min., Vector4 Max.)|  Gibt einen Vector4 zurück, der die arretierten Werte für die jeweilige Komponente enthält|
|Max (Vector4-Wert1 Vector4-Wert2)|   Gibt einen Vector4 zurück, der für jede entsprechende Komponente von Wert1 und Wert2 ein Max. durchgeführt hat|
|Min (Vector4-Wert1 Vector4-Wert2)|   Gibt einen Vector4 zurück, die für jede entsprechende Komponente von Wert1 und Wert2 ein Min. durchgeführt hat|
|Scale(Vector3-Wert, Float-Faktor)|    Gibt einen Vector3 zurück, wobei jede Komponente des Vektors mit dem Skalierungsfaktor multipliziert wurde.|
|Transform(Vector4-Wert, Matrix4x4-Matrix)|    Gibt einen Vector4 zurück, der sich aus der linearen Transformation zwischen einem Vector4 und einer Matrix4x4 ergibt (auch bekannt als Multiplizieren eines Vektors mit einer Matrix).|
|Lerp(Vector4-Wert1, Vector4-Wert2, Float-Fortschritt)|  Gibt einen Vector4 zurück, der die berechnete lineare Interpolation zwischen den beiden Vector4-Werten basierend auf dem Fortschritt darstellt (Hinweis: Fortschritt ist zwischen 0,0 und 1,0)|
|Length(Vector4-Wert)| Gibt einen Float-Wert zurück, der die Länge/Größe von Vector4 darstellt|
|LengthSquared(Vector4)|    Gibt einen Float-Wert zurück, der das Quadrat der Länge/Größe eines Vector4 darstellt|
|Distance(Vector4-Wert1, Vector4-Wert2)|  Gibt einen Float-Wert zurück, der den Abstand zwischen zwei Vector4-Werten darstellt|
|DistanceSquared(Vector4-Wert1, Vector4-Wert2)|   Gibt einen Float-Wert zurück, der das Quadrat zwischen zwei Vector4-Werten darstellt|
|Normalize(Vector4-Wert)|  Gibt einen Vector4-Wert zurück, der den Einheitsvektor des Parameters darstellt, wobei alle Komponenten normalisiert wurden|
|Vector4(Float x, Float y, Float z, Float w)|   Erstellt eine Vector4 mit vier Float-Parametern|

###Matrix3x2  

|Funktion und Konstruktorvorgänge|   Beschreibung|
|-----------------------------------|--------------|
|Scale(Matrix3x2-Wert, Float-Faktor)|  Gibt eine Matrix3x2 zurück, wobei jede Komponente der Matrix mit dem Skalierungsfaktor multipliziert wurde.|
|Inverse(Matrix3x2-Wert)| Gibt ein Matrix3x2-Objekt zurück, das die reziproke Matrix darstellt|
|Lerp(Matrix3x2-Wert1, Matrix3x2-Wert2, Float-Fortschritt)|  Gibt eine Matrix3x2 zurück, welche die berechnete lineare Interpolation zwischen den beiden Matrix3x2-Werten basierend auf dem Fortschritt darstellt (Hinweis: Fortschritt ist zwischen 0,0 und 1,0)|
|Matrix3x2(Float M11, Float M12, Float M21, Float M22, Float M31, Float M32)|   Erstellt eine Matrix3x2 mit 6 Float-Parametern|
|Matrix3x2.CreateFromScale(Vector2-Skalierung)|  Erstellt eine Matrix3x2 aus einem Vector2, die Skalierung darstellt<br/>\[scale.X, 0.0<br/> 0.0, scale.Y<br/> 0.0, 0.0 \]|
|Matrix3x2.CreateFromTranslation(Vector2-Übersetzung)|  Erstellt eine Matrix3x2 aus einem Vector2, die Übersetzung darstellt<br/>\[1.0, 0.0,<br/> 0.0, 1.0,<br/> translation.X, translation.Y\]|
    
###Matrix4x4  

|Funktion und Konstruktorvorgänge|   Beschreibung|
|-----------------------------------|--------------|
|Scale(Matrix4x4-Wert, Float-Faktor)|  Gibt eine Matrix4x4 zurück, wobei jede Komponente der Matrix mit dem Skalierungsfaktor multipliziert wurde.|
|Inverse(Matrix4x4)|    Gibt ein Matrix4x4-Objekt zurück, das die reziproke Matrix darstellt|
|Lerp(Matrix4x4-Wert1, Matrix4x4-Wert2, Float-Fortschritt)|  Gibt eine Matrix4x4 zurück, welche die berechnete lineare Interpolation zwischen den beiden Matrix4x4-Werten basierend auf dem Fortschritt darstellt (Hinweis: Fortschritt ist zwischen 0,0 und 1,0)|
|Matrix4x4(Float M11, Float M12, Float M13, Float M14,<br/>Float M21, Float M22, Float M23, Float M24,<br/>    Float M31, Float M32, Float M33, Float M34,<br/>    Float M41, Float M42, Float M43, Float M44)| Erstellt eine Matrix4x4 mit 16 Float-Parametern|
|Matrix4x4.CreateFromScale(Vector3-Skalierung)|  Erstellt eine Matrix4x4 aus einem Vektor3, die Skalierung darstellt<br/>\[scale.X, 0.0, 0.0, 0.0,<br/> 0.0, scale.Y, 0.0, 0.0,<br/> 0.0, 0.0, scale.Z, 0.0,<br/> 0.0, 0.0, 0.0, 1.0\]|
|Matrix4x4.CreateFromTranslation(Vector3-Übersetzung)|  Erstellt eine Matrix4x4 aus einem Vector3, die Übersetzung darstellt<br/>\[1.0, 0.0, 0.0, 0.0,<br/> 0.0, 1.0, 0.0, 0.0,<br/> 0.0, 0.0, 1.0, 0.0,<br/> translation.X, translation.Y, translation.Z, 1.0\]|
|Matrix4x4.CreateFromAxisAngle(Vector3-Achse, Float-Winkel)|  Erstellt eine Matrix4x4 aus einer Vektor3-Achse und einem Float, die einen Winkel darstellt|

###Quaternion  

|Funktion und Konstruktorvorgänge|   Beschreibung|
|-----------------------------------|--------------|
|Slerp(Quaternion-Wert1, Quaternion-Wert2, Float-Fortschritt)|   Gibt einen Quaternion-Wert zurück, der die berechnete sphärische Interpolation zwischen den beiden Quaternion-Werten basierend auf dem Fortschritt darstellt (Hinweis: Fortschritt ist zwischen 0,0 und 1,0)|
|Concatenate(Quaternion-Wert1 Quaternion-Wert2)|  Gibt einen Quaternion-Wert zurück, der eine Verkettung von zwei Quaternionen darstellt (auch bekannt als eine Quaternion, die eine Kombination aus zwei einzelnen Drehungen darstellt)|
|Length(Quaternion-Wert)|  Gibt einen Float-Wert zurück, der die Länge/Größe des Quaternions darstellt.|
|LengthSquared(Quaternion)| Gibt einen Float-Wert zurück, der das Quadrat der Länge/Größe eines Quaternions darstellt|
|Normalize(Quaternion-Wert)|   Gibt eine Quaternion zurück, deren Komponenten normalisiert wurden|
|Quaternion.CreateFromAxisAngle(Vector3-Achse, skalarer Winkel)|    Erstellt eine Quaternion aus einer Vektor3-Achse und einem Skalar, die einen Winkel darstellt|
|Quaternion(Float x, Float y, Float z, Float w)|    Erstellt eine Quaternion aus vier Float-Werten|

###Color

|Funktion und Konstruktorvorgänge|   Beschreibung|
|-----------------------------------|--------------|
|ColorLerp(Farbe FarbeZu, Farbe FarbeVon, Float-Fortschritt)| Gibt ein Color-Objekt zurück, das den berechneten linearen Interpolationswert zwischen zwei Color-Objekten basierend auf einem bestimmten Fortschritt darstellt. (Hinweis: Fortschritt ist zwischen 0,0 und 1,0)|
|ColorLerpRGB(Farbe FarbeZu, Farbe FarbeVon, Float-Fortschritt)|  Gibt ein Color-Objekt zurück, das den berechneten linearen Interpolationswert zwischen zwei Objekten basierend auf einem bestimmten Fortschritt im RGB-Farbraum darstellt.|
|ColorLerpHSL(Farbe FarbeZu, Farbe FarbeVon, Float-Fortschritt)|  Gibt ein Color-Objekt zurück, das den berechneten linearen Interpolationswert zwischen zwei Objekten basierend auf einem bestimmten Fortschritt im HSL-Farbraum darstellt.|
|ColorArgb(Float a, Float r, Float g, Float b)| Erstellt ein Objekt, das durch ARGB-Komponenten definierte Farbe darstellt|
|ColorHsl(Float h, Float s, Float l)|   Erstellt ein Objekt, das durch HSL-Komponenten definierte Farbe darstellt (Hinweis: Farbton wird von 0 und 2pi definiert)|







<!--HONumber=Jul16_HO1-->


