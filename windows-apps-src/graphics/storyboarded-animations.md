---
author: Jwmsft
ms.assetid: 0CBCEEA0-2B0E-44A1-A09A-F7A939632F3A
title: Storyboardanimationen
description: Storyboardanimationen sind nicht nur Animationen visueller Art.
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 361765de700af2a701e16fc27a5867d80907865a

---
# Storyboardanimationen

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Storyboardanimationen sind nicht nur Animationen visueller Art. Eine Storyboardanimation bietet die Möglichkeit, den Wert einer Abhängigkeitseigenschaft zeitabhängig zu ändern. Einer der Hauptgründe für eine Storyboardanimation, die nicht aus der Animationsbibliothek stammt, ist die Definition des visuellen Zustands für ein Steuerelement als Teil einer Steuerelementvorlage oder Seitendefinition.

## Unterschiede zu Silverlight und WPF

Lesen Sie diesen Abschnitt, wenn Sie mit Microsoft Silverlight or Windows Presentation Foundation (WPF) vertraut sind. Ansonsten können Sie diesen Abschnitt überspringen.

Grundsätzlich werden Storyboardanimationen in Windows-Runtime-Apps wie in Silverlight oder WPF erstellt. Es gibt jedoch einige wichtige Unterschiede:

-   Storyboardanimationen sind nicht die einzige Möglichkeit zum visuellen Animieren einer UI und stellen in dieser Beziehung auch nicht unbedingt die einfachste Möglichkeit für Entwickler dar. Anstelle von Storyboardanimationen empfiehlt sich als bessere Entwurfsalternative häufig die Verwendung von Design- und Übergangsanimationen. Dabei können die empfohlenen UI-Animationen schnell erstellt werden, ohne sich mit den Details der Zielgruppenadressierung für Animationseigenschaften auseinandersetzen zu müssen. Weitere Informationen finden Sie unter [Übersicht über Animationen](animations-overview.md).
-   In der Windows-Runtime enthalten viele XAML-Steuerelemente im Rahmen des integrierten Verhaltens Design- und Übergangsanimationen. WPF- und Silverlight-Steuerelemente haben bisher meist nicht über ein Standardverhalten für Animationen verfügt.
-   Nicht alle von Ihnen erstellten benutzerdefinierten Animationen können standardmäßig in einer Windows-Runtime-App ausgeführt werden, wenn das Animationssystem ermittelt, dass die Animation die Leistung Ihrer Benutzeroberfläche beeinträchtigen kann. Animationen, für die das System eine mögliche Leistungsbeeinträchtigung erkennt, werden als *abhängige Animationen* bezeichnet. Sie sind abhängig, weil die Taktung Ihrer Animationen direkt mit dem UI-Thread arbeitet, wo auch aktive Benutzereingaben und andere Aktualisierungen versuchen, die Laufzeitänderungen auf die Benutzeroberfläche anzuwenden. Eine abhängige Animation, die sehr viele Systemressourcen im UI-Thread verbraucht, kann dafür sorgen, dass eine App in bestimmten Situationen scheinbar nicht mehr reagiert. Wenn eine Animation Layoutänderungen bedingt oder die Leistung im UI-Thread potenziell auf andere Weise beeinträchtigen kann, müssen Sie die Animation häufig explizit aktivieren, um die Ausführung zu ermöglichen. Hierzu gibt es die **EnableDependentAnimation**-Eigenschaft für bestimmte Animationsklassen. Weitere Informationen finden Sie unter [Abhängige und unabhängige Animationen](./storyboarded-animations.md#dependent-and-independent-animations).
-   Benutzerdefinierte Beschleunigungsfunktionen werden in der Windows-Runtime derzeit nicht unterstützt.

## Definieren von Storyboardanimationen

Eine Storyboardanimation bietet die Möglichkeit, den Wert einer Abhängigkeitseigenschaft zeitabhängig zu ändern. Die Eigenschaft, die Sie animieren, ist nicht immer eine Eigenschaft, die sich direkt auf die UI der App auswirkt. Da es bei XAML jedoch um das Definieren einer UI für eine App geht, ist es in der Regel eine UI-bezogene Eigenschaft, die Sie animieren. Beispielsweise können Sie den Winkel eines [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932)-Elements oder den Farbwert eines Schaltflächenhintergrunds animieren.

Einer der Hauptgründe für die Definition einer Storyboardanimation ist, wenn Sie als Steuerelementautor oder beim Erstellen einer neuen Vorlage für ein Steuerelement visuelle Zustände definieren. Weitere Informationen finden Sie unter [Storyboardanimationen für visuelle Zustände](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

Die Konzepte und APIs für Storyboardanimationen, die in diesem Thema beschrieben werden, gelten unabhängig davon, ob Sie visuelle Zustände oder eine benutzerdefinierte Animation für eine App definieren.

Damit eine Eigenschaft animiert werden kann, muss es sich bei der Eigenschaft, auf die Sie mit einer Storyboardanimation abzielen, um eine *Abhängigkeitseigenschaft* handeln. Eine Abhängigkeitseigenschaft ist ein wichtiges Feature der Windows-Runtime-XAML-Implementierung. Die schreibbaren Eigenschaften der häufigsten UI-Elemente werden normalerweise als Abhängigkeitseigenschaften implementiert, damit Sie diese animieren, datengebundene Werte anwenden oder ein [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849)-Element anwenden und mit einem [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817)-Element auf die Eigenschaft verweisen können. Weitere Informationen zur Funktionsweise von Abhängigkeitseigenschaften finden Sie unter [Übersicht über Abhängigkeitseigenschaften](https://msdn.microsoft.com/library/windows/apps/Mt185583).

In den meisten Fällen definieren Sie eine Storyboardanimation, indem Sie XAML-Code schreiben. Wenn Sie ein Tool wie Microsoft Visual Studio verwenden, können Sie den XAML-Code vom Tool erzeugen lassen. Es ist auch möglich, eine Storyboardanimation mithilfe von normalem Code zu definieren. Dies ist jedoch keine gängige Lösung.

Sehen wir uns dazu ein einfaches Beispiel an. In diesem XAML-Beispiel wird die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962)-Eigenschaft auf einem bestimmten [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)-Objekt animiert.

```xml
<!-- Animates the rectangle's opacity. -->
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation
              Storyboard.TargetName="MyAnimatedRectangle"
              Storyboard.TargetProperty="Opacity"
              From="1.0" To="0.0" Duration="0:0:1"/>
        </Storyboard>
// a different area of the XAML
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
```
      
### Angeben des zu animierenden Objekts

Im vorherigen Beispiel hat das Storyboard die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962)-Eigenschaft eines [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)-Objekts animiert. Dabei deklarieren Sie nicht die Animationen des Objekts selbst. Stattdessen führen Sie dies in der Animationsdefinition eines Storyboards durch. Storyboards werden normalerweise in XAML-Code definiert, der sich nicht in unmittelbarer Nähe der XAML-UI-Definition des zu animierenden Objekts befindet. Stattdessen werden sie meist als XAML-Ressource eingerichtet.

Um eine Animation mit einem Ziel zu verbinden, verweisen Sie anhand des identifizierenden Programmierungsnamens auf das Ziel. Sie sollten stets das [x:Name-Attribut](https://msdn.microsoft.com/library/windows/apps/Mt204788) in der XAML-UI-Definition anwenden, um das Objekt zu benennen, das Sie animieren möchten. Anschließend geben Sie das zu animierende Objekt als Ziel an, indem Sie [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/Hh759823) in der Animationsdefinition festlegen. Für den Wert von **Storyboard.TargetName** verwenden Sie die Namenszeichenfolge des Zielobjekts. Dies ist die Zeichenfolge, die Sie bereits an anderer Stelle mit dem x:Name-Attribut festgelegt haben.

### Angeben der zu animierenden Abhängigkeitseigenschaft

Sie legen einen Wert für [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824) in der Animation fest. Damit wird bestimmt, welche spezifische Eigenschaft des Zielobjekts animiert wird.

Es kann vorkommen, dass Sie eine Eigenschaft als Ziel angeben müssen, bei der es sich nicht um eine direkte Eigenschaft des Zielobjekts handelt, sondern die tiefer in einer Beziehung zwischen Objekt und Eigenschaft geschachtelt ist. Dies ist häufig erforderlich, um Detailinformationen für eine Gruppe von beitragenden Objekt- und Eigenschaftswerten anzuzeigen, bis Sie auf einen Eigenschaftstyp verweisen können, der animiert werden kann ([**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870), [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)). Dieses Konzept wird als *indirekte Zielbestimmung* bezeichnet. Um eine Eigenschaft auf diese Art als Ziel auszuwählen, wird ein *Eigenschaftspfad* verwendet.

Hier sehen Sie ein Beispiel. Ein häufiges Szenario für eine Storyboardanimation ist eine Farbänderung für einen Teil einer App-UI oder eines Steuerelements, um anzuzeigen, dass sich das Steuerelement in einem bestimmten Zustand befindet. Angenommen, Sie möchten das [**Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665)-Element eines [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)-Elements so animieren, dass sich die Farbe von Rot in Grün ändert. Wenn Sie erwarten, dass hierbei ein [**ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066)-Element verwendet wird, liegen Sie richtig. Keine der Eigenschaften von UI-Elementen, die sich auf die Farbe des Objekts auswirken, weisen jedoch den Typ [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) auf. Stattdessen weisen sie den Typ [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) auf. Das eigentliche Ziel der Animation sollte also die [**Color**](https://msdn.microsoft.com/library/windows/apps/BR242963)-Eigenschaft der [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)-Klasse sein. Dabei handelt es sich um einen von **Brush** abgeleiteten Typ, der normalerweise für diese farbbezogenen UI-Eigenschaften verwendet wird. In Bezug auf die Erstellung eines Eigenschaftspfads für die Eigenschaftsangabe der Animation sieht dies wie folgt aus:

```xml
<Storyboard x:Name="myStoryboard">
    <ColorAnimation Storyboard.TargetName="tb1" From="Red" To="Green"
      Storyboard.TargetProperty="(TextBlock.Foreground).(SolidColorBrush.Color)"
   />
    </Storyboard>
```

Stellen Sie sich die Syntax hinsichtlich ihrer Komponenten folgendermaßen vor:

-   Jedes Klammerpaar "()" umschließt jeweils einen Eigenschaftsnamen.
-   Der Eigenschaftsname enthält einen Punkt zur Trennung eines Typnamens und eines Eigenschaftsnamens, damit die angegebene Eigenschaft eindeutig ist.
-   Der Punkt in der Mitte, der sich nicht innerhalb der Klammern befindet, ist ein Schritt. Dies wird von der Syntax wie folgt interpretiert: Verwende den Wert der ersten Eigenschaft (ein Objekt), greife auf sein Objektmodell zu und gib eine bestimmte Untereigenschaft des Werts der ersten Eigenschaft an.

Unten ist eine Liste mit Szenarien zum Angeben von Animationszielen aufgeführt, bei denen Sie Eigenschaften normalerweise indirekt angeben. Außerdem sind einige Eigenschaftspfadzeichenfolgen aufgeführt, die an die zu verwendende Syntax angenähert sind:

-   Animation des [**X**](https://msdn.microsoft.com/library/windows/apps/BR243029)-Werts eines [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)-Elements bei Anwendung auf [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980): `(UIElement.RenderTransform).(TranslateTransform.X)`
-   Animation eines [**Color**](https://msdn.microsoft.com/library/windows/apps/BR242963)-Elements innerhalb eines [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078)-Elements eines [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108)-Elements bei Anwendung auf [**Fill**](https://msdn.microsoft.com/library/windows/apps/BR243378): `(Shape.Fill).(GradientBrush.GradientStops)[0].(GradientStop.Color)`
-   Animation des [**X**](https://msdn.microsoft.com/library/windows/apps/BR243029)-Werts eines [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)-Elements, das 1 von 4 Transformationen in einem [**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022)-Element darstellt, bei Anwendung auf [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980):`(UIElement.RenderTransform).(TransformGroup.Children)[3].(TranslateTransform.X)`

Sie sehen, dass Zahlen in einigen Beispielen in eckige Klammern gesetzt sind. Dabei handelt es sich um einen Indexer. Damit wird angegeben, dass der davor stehende Eigenschaftsname als Wert eine Sammlung aufweist und dass Sie ein Element dieser Sammlung (Angabe per nullbasiertem Index) verwenden möchten.

Sie können auch angefügte XAML-Eigenschaften animieren. Setzen Sie den vollständigen Namen der angefügten Eigenschaft stets in Klammern, z.B. `(Canvas.Left)`. Weitere Informationen finden Sie unter [Animieren von angefügten XAML-Eigenschaften](./storyboarded-animations.md#animating-xaml-attached-properties).

Weitere Informationen zur Verwendung eines Eigenschaftenpfads für die indirekte Auswahl der zu animierenden Eigenschaft finden Sie unter [Property-path-Syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586) oder [**Angefügte Storyboard.TargetProperty-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/Hh759824).

### Animationstypen

Das Windows-Runtime-Animationssystem verfügt über drei spezielle Typen für Storyboardanimationen:

-   [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) kann mit jeder [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) animiert werden.
-   [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) kann mit jeder [**PointAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210346) animiert werden.
-   [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) kann mit jeder [**ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066) animiert werden.

Es ist auch ein generalisierter [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx)-Animationstyp für Objektverweiswerte vorhanden, der später erläutert wird.

### Angeben der animierten Werte

Bisher wurde beschrieben, wie Sie das Objekt und die Eigenschaft für die Animation angeben, aber es wurde noch nicht erläutert, wie sich die Animation während der Ausführung auf den Eigenschaftswert auswirkt.

Die beschriebenen Animationstypen werden auch als **From**/**To**/**By**-Animationen bezeichnet. Dies bedeutet, dass die Animation den Wert einer Eigenschaft in Abhängigkeit der Zeit ändert, indem eine oder mehrere dieser Eingaben aus der Animationsdefinition verwendet werden:

-   Der Wert beginnt beim **From**-Wert. Wenn Sie keinen **From**-Wert angeben, wird als Startwert der Wert genutzt, den die animierte Eigenschaft jeweils vor dem Ausführen der Animation aufweist. Dies kann ein Standardwert, ein Wert aus einem Stil oder einer Vorlage oder ein Wert sein, der speziell von einer XAML-UI-Definition oder einem App-Code angewendet wird.
-   Am Ende der Animation ist der Wert der **To**-Wert.
-   Sie können auch die **By**-Eigenschaft festlegen, um einen Endwert relativ zum Startwert anzugeben. Legen Sie diese Eigenschaft dazu anstelle der **To**-Eigenschaft fest.
-   Wenn Sie keinen **To**-Wert oder einen **By**-Wert angeben, wird als Endwert der Wert verwendet, den die animierte Eigenschaft vor dem Ausführen der Animation aufweist. In diesem Fall empfiehlt sich die Nutzung eines **From**-Werts, weil die Animation den Wert ansonsten gar nicht ändert, da der Start- und Endwert identisch sind.
-   Eine Animation verfügt normalerweise über mindestens ein **From**-, **By**- oder **To**-Element, aber nicht über alle drei.

Sehen wir uns das vorherige XAML-Beispiel und die **From**- und **To**-Werte sowie **Duration** erneut an. Im Beispiel wird die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962)-Eigenschaft animiert, und der Eigenschaftstyp von **Opacity** lautet[**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Als Animation muss hier also [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) verwendet werden.

`From="1.0" To="0.0"` Gibt an, dass die [**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962)-Eigenschaft während der Ausführung der Animation mit dem Wert „1“ beginnt und zu „0“ animiert wird. Anders ausgedrückt: Hinsichtlich der Bedeutung dieser [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)-Werte für die **Opacity**-Eigenschaft bewirkt diese Animation, dass das Objekt undurchsichtig gestartet wird und dann transparent wird.

```xml
...
    <Storyboard x:Name="myStoryboard">
    <DoubleAnimation
    Storyboard.TargetName="MyAnimatedRectangle"
    Storyboard.TargetProperty="Opacity"
    From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
...
```

`Duration="0:0:1"` gibt die Dauer der Animation an, d.h. die Geschwindigkeit, mit der das Rechteck verblasst. Eine [ **Duration** ](https://msdn.microsoft.com/library/windows/apps/BR243207)-Eigenschaft wird in der Form *Stunden*:*Minuten*:*Sekunden* angegeben. Die Zeitdauer in diesem Beispiel beträgt eine Sekunde.

Weitere Informationen zu [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)-Werten und der XAML-Syntax finden Sie unter [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377).

**Hinweis:** Wenn Sie sich in Bezug auf das gezeigte Beispiel sicher sind, dass der Startzustand des animierten Objekts für [**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962) stets den Wert„1“ aufweist (durch standardmäßige oder explizite Festlegung), können Sie den **From**-Wert auslassen. Die Animation verwendet dann den impliziten Startwert, und das Ergebnis ist identisch.

 

### From/To/By akzeptieren NULL-Werte

Es wurde bereits erwähnt, dass Sie **From**, **To** oder **By** weglassen können und so aktuelle nicht animierte Werte als Ersatzwerte für einen fehlenden Wert verwenden können. **From**, **To** oder **By**-Eigenschaften einer Animation sind möglicherweise von einem anderen Typ, als Sie vermuten. Der Typ der [**DoubleAnimation.To**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx)-Eigenschaft lautet beispielsweise nicht [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Stattdessen gilt [**Nullable**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx) für **Double**. Der Standardwert lautet **null**, nicht0. Anhand dieses **null**-Werts kann das Animationssystem unterscheiden, dass Sie keinen spezifischen Wert für eine **From**-, **To**- oder **By**-Eigenschaft festgelegt haben. VisualC++-Komponentenerweiterungen (C++/CX) verfügen nicht über einen **Nullable**-Typ und nutzen stattdessen [**IReference**](https://msdn.microsoft.com/library/windows/apps/BR225864).

### Andere Eigenschaften einer Animation

Die nächsten in diesem Abschnitt beschriebenen Eigenschaften sind alle optional, da sie Standardwerte aufweisen, die für die meisten Animationen geeignet sind.

### **AutoReverse**

Wenn Sie für eine Animation entweder [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) oder [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) angeben, wird die Animation einmal ausgeführt, und als Dauer wird die Angabe unter [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) verwendet.

Mit der [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202)-Eigenschaft wird angegeben, ob eine Zeitachse rückwärts wiedergegeben wird, nachdem das Ende von [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) erreicht wurde. Wenn Sie diese Eigenschaft auf **true** festlegen, wird die Animation nach dem Erreichen des Endes ihrer deklarierten Dauer ([**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)) umgekehrt, sodass der Wert von seinem Endwert (**To**) wieder in den Startwert (**From**) geändert wird. Dies bedeutet, dass die Animation praktisch doppelt so lange wie im [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)-Element angegeben ausgeführt wird.

### **RepeatBehavior**

Mit der [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211)-Eigenschaft wird entweder festgelegt, wie häufig eine Zeitachse wiedergegeben wird, oder eine längere Dauer, während der die Zeitachse wiederholt wird. Standardmäßig gilt für eine Zeitachse ein Durchlaufwert von "1x". Dies bedeutet, dass sie gemäß dem [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)-Element einmal ausgeführt und nicht wiederholt wird.

Sie können auch angeben, dass die Animation mehrfach durchlaufen werden soll. Mit dem Wert "3x" erreichen Sie z.B., dass die Animation dreimal ausgeführt wird. Alternativ dazu können Sie eine andere Dauer ([**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)) für [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) angeben. Es ist ratsam, diesen **Duration**-Wert auf einen längeren Wert als den **Duration**-Wert der Animation selbst festzulegen. Wenn Sie z.B. für **RepeatBehavior** den Wert "0:0:10" festlegen und die Animation für [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) den Wert "0:0:2" aufweist, wird die Animation fünfmal wiederholt. Falls diese Werte nicht ohne Rest teilbar sind, wird die Animation an dem Punkt abgeschnitten, an dem der **RepeatBehavior**-Zeitpunkt erreicht wird, also z.B. auch nach der Hälfte der Animation. Außerdem haben Sie noch die Möglichkeit, den Spezialwert "Forever" anzugeben, bei dem die Animation endlos lange ausgeführt wird, bis sie explizit beendet wird.

Weitere Informationen zu [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411)-Werten und zur XAML-Syntax finden Sie unter [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411).

### **FillBehavior="Stop"**

Wenn eine Animation endet, belässt die Animation den Eigenschaftswert standardmäßig auf dem letzten per **To** oder **By** geänderten Wert. Dies gilt auch, wenn die Dauer abgelaufen ist. Wenn Sie den Wert der [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209)-Eigenschaft jedoch auf [**FillBehavior.Stop**](https://msdn.microsoft.com/library/windows/apps/BR210306) festlegen, wird der Wert des animierten Werts auf den Stand vor dem Anwenden der Animation zurückgesetzt. Genauer gesagt: Er wird auf den aktuell geltenden Wert zurückgesetzt, der vom Abhängigkeitseigenschaftensystem bestimmt wird. (Weitere Informationen zu dieser Unterscheidung finden Sie unter [Übersicht über Abhängigkeitseigenschaften](https://msdn.microsoft.com/library/windows/apps/Mt185583).)

### **BeginTime**

Standardmäßig lautet die [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204) einer Animation "0:0:0". Die Animation beginnt also, sobald das enthaltende [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) ausgeführt wird. Sie können dies ändern, falls das **Storyboard** mehr als eine Animation enthält und Sie die Startzeiten der anderen Animationen gegenüber der ersten Animation staffeln möchten. Außerdem können Sie auch eine absichtliche kurze Verzögerung einbauen.

### **SpeedRatio**

Wenn Sie mehr als eine Animation in einem [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) verwenden, können Sie die Zeitrate für eine oder mehrere Animationen relativ zum **Storyboard** ändern. Letztendlich wird über das übergeordnete **Storyboard** gesteuert, wie die Zeitdauer ([**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)) während der Ausführung der Animationen verstreicht. Diese Eigenschaft wird nicht sehr häufig verwendet. Weitere Informationen finden Sie unter [**SpeedRatio**](https://msdn.microsoft.com/library/windows/apps/BR243213).

## Definieren von mehr als einer Animation in einem **Storyboard**

Ein [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) kann mehr als eine Animationsdefinition enthalten. Möglicherweise verwenden Sie z. B. mehr als eine Animation, wenn Sie verwandte Animationen auf zwei Eigenschaften desselben Zielobjekts anwenden. Sie können zum Beispiel die [**TranslateX**](https://msdn.microsoft.com/library/windows/apps/BR228122)- und [**TranslateY**](https://msdn.microsoft.com/library/windows/apps/BR228124)-Eigenschaft eines [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)-Elements ändern, das als [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)-Element eines Steuerelements der UI verwendet wird. Dies führt dazu, dass das Element diagonal übersetzt wird. Dafür benötigen Sie zwei unterschiedliche Animationen. Es kann jedoch ratsam sein, Animationen zu nutzen, die Teil desselben **Storyboard**-Elements sind, weil die beiden Animationen immer zusammen ausgeführt werden sollen.

Die Animationen müssen nicht denselben Typ oder dasselbe Objekt als Ziel aufweisen. Ihre Dauer kann unterschiedlich sein, und es ist keine gemeinsame Nutzung von Eigenschaftswerten erforderlich.

Wenn das übergeordnete [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) ausgeführt wird, werden auch die beiden Animationen ausgeführt.

Die [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)-Klasse verfügt zudem über viele gleiche Animationseigenschaften wie die Animationstypen, weil von beiden die [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517)-Basisklasse verwendet wird. Ein **Storyboard** kann also ein [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211)-Element oder ein [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204)-Element aufweisen. Sie legen diese Elemente jedoch normalerweise nicht in einem **Storyboard** fest, es sei denn, alle darin enthaltenen Animationen sollen das entsprechende Verhalten aufweisen. Es gilt die allgemeine Regel, dass jede **Timeline**-Eigenschaft, die in einem **Storyboard** festgelegt wird, für alle untergeordneten Animationen gilt. Wenn hierfür nichts festgelegt wird, hat das **Storyboard** eine implizite Dauer, die aus dem längsten [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)-Wert der enthaltenen Animationen berechnet wird. Ein explizit festgelegtes [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)-Element in einem **Storyboard**, das kürzer als eine seiner untergeordneten Animationen ist, führt dazu, dass die Animation abgeschnitten wird. Dies ist normalerweise nicht wünschenswert.

Ein Storyboard kann nicht zwei Animationen enthalten, die jeweils versuchen, dieselbe Eigenschaft desselben Objekts anzusprechen und zu animieren. Wenn Sie dies versuchen, tritt beim Ausführen des Storyboards ein Laufzeitfehler auf. Diese Einschränkung gilt auch, wenn sich die Animationen zeitlich nicht überlappen, weil absichtlich unterschiedliche [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204)-Werte und Dauern verwendet werden. Falls Sie in einem Storyboard wirklich eine komplexere Animationszeitachse auf eine Eigenschaft anwenden möchten, können Sie eine Keyframeanimation nutzen. Siehe [Keyframeanimationen und Animationen für Beschleunigungsfunktionen](key-frame-and-easing-function-animations.md).

Das Animationssystem kann mehr als eine Animation auf den Wert einer Eigenschaft anwenden, sofern diese Eingaben von mehreren Storyboards stammen. Dieses Verhalten wird für gleichzeitig ausgeführte Storyboards in der Regel nicht verwendet. Mithilfe einer von einer App definierten Animation, die Sie auf eine Steuerelementeigenschaft anwenden, kann jedoch der **HoldEnd**-Wert einer Animation geändert werden, die als Teil des Modells für den visuellen Zustand des Steuerelements bereits ausgeführt wurde.

## Definieren eines Storyboards als Ressource

Ein [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) ist der Container, in den Sie Animationsobjekte einschließen. Normalerweise definieren Sie das **Storyboard** als Ressource, die für das zu animierende Objekt verfügbar ist, und zwar entweder unter [**Resources**](https://msdn.microsoft.com/library/windows/apps/BR208740) auf Seitenebene oder unter [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/BR242338).

Im nächsten Beispiel wird veranschaulicht, wie das [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) aus dem vorherigen Beispiel in eine [**Resources**](https://msdn.microsoft.com/library/windows/apps/BR208740)-Definition auf Seitenebene eingeschlossen wird, bei der das **Storyboard** eine Ressource mit Schlüssel des [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503)-Stammelements darstellt. Beachten Sie das [x:Name-Attribut](https://msdn.microsoft.com/library/windows/apps/Mt204788). Mit diesem Attribut definieren Sie einen Variablennamen für das **Storyboard**, damit andere Elemente im XAML-Code und in normalem Code später auf das **Storyboard** verweisen können.

```xml
<Page ...>
  <Page.Resources>
        <!-- Storyboard resource: Animates a rectangle's opacity. -->
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation
              Storyboard.TargetName="MyAnimatedRectangle"
              Storyboard.TargetProperty="Opacity"
              From="1.0" To="0.0" Duration="0:0:1"/>
        </Storyboard>
  </Page.Resources>
  <!--Page root element, UI definition-->
  <StackPanel>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </StackPanel>
</Page>
```

Das Definieren von Ressourcen auf der XAML-Stammebene einer XAML-Datei, z.B. page.xaml oder app.xaml, ist eine übliche Vorgehensweise zum Organisieren von Ressourcen mit Schlüsseln in XAML. Sie können Ressourcen auch auf separate Dateien aufteilen und diese in Apps oder Seiten zusammenführen. Weitere Informationen finden Sie unter [ResourceDictionary- und XAML-Ressourcenreferenzen](https://msdn.microsoft.com/library/windows/apps/Mt187273).

**Hinweis:**  Windows-Runtime-XAML unterstützt die Identifizierung von Ressourcen unter Verwendung des [x:Key-Attributs](https://msdn.microsoft.com/library/windows/apps/Mt204787) oder des [x:Name-Attributs](https://msdn.microsoft.com/library/windows/apps/Mt204788). Das x:Name-Attribut wird für ein [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) häufiger verwendet, weil darauf später anhand des Variablennamens verwiesen werden soll, damit Sie dessen [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin)-Methode aufrufen und die Animationen ausführen können. Wenn Sie dagegen das [x:Key-Attribut](https://msdn.microsoft.com/library/windows/apps/Mt204787) verwenden, müssen Sie [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794)-Methoden wie den [**Item**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.item)-Indexer zum Abrufen als Ressource mit Schlüssel nutzen und das abgerufene Objekt dann für das **Storyboard** umwandeln, um die **Storyboard**-Methoden zu verwenden.

 

Sie können Ihre Animationen auch in einer [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)-Einheit platzieren, wenn Sie die Ansichtszustandsanimationen für die visuelle Gestaltung eines Steuerelements deklarieren. In diesem Fall kommen die definierten **Storyboard**-Elemente in einen [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007)-Container, der tiefer in einem [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849) verschachtelt ist (der **Style** ist die Schlüsselressource). Sie benötigen in diesem Fall keinen Schlüssel oder Namen für **Storyboard**, da **VisualState** einen Zielnamen besitzt, den [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager) aufrufen kann. Die Stile für Steuerelemente werden häufig auf separate XAML-[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794)-Dateien aufgeteilt, statt dass sie in eine Seiten- oder App-**Ressources**-Sammlung platziert werden. Weitere Informationen finden Sie unter [Storyboardanimationen für visuelle Zustände](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

## Abhängige und unabhängige Animationen

An diesem Punkt ist es sinnvoll, einige wichtige Aspekte zur Funktionsweise des Animationssystems zu erläutern. Besonders wichtig ist, dass sich die grundlegende Interaktion der Animation danach richtet, wie eine Windows-Runtime-App auf dem Bildschirm gerendert wird und wie beim Rendern Verarbeitungsthreads eingesetzt werden. Eine Windows-Runtime-App verfügt immer über einen UI-Hauptthread, der für die Aktualisierung des Bildschirms mit den aktuellen Informationen zuständig ist. Außerdem verfügt eine Windows-Runtime-App über einen Kompositionsthread, der zum Vorabberechnen von Layouts unmittelbar vor dem Anzeigen verwendet wird. Wenn Sie die UI animieren, kann dies für den UI-Thread einen erhöhten Arbeitsaufwand bedeuten. Das System muss große Bereiche des Bildschirms neu zeichnen, wobei relativ kurze Zeitintervalle zwischen den Aktualisierungen verwendet werden. Dies ist erforderlich, um den aktuellen Eigenschaftswert der animierten Eigenschaft zu erfassen. Wenn Sie dabei nicht sorgfältig vorgehen, besteht das Risiko, dass die Reaktionsfähigkeit der UI aufgrund einer Animation sinkt. Außerdem kann die Leistung anderer App-Features beeinträchtigt werden, die sich ebenfalls auf demselben UI-Thread befinden.

Die Animationsvariante, für die das Risiko einer Verlangsamung des UI-Threads ermittelt wird, wird als *abhängige Animation* bezeichnet. Eine Animation, für die dieses Risiko nicht besteht, ist eine *unabhängige Animation*. Die Unterscheidung zwischen abhängigen und unabhängigen Animationen basiert nicht nur auf den Animationstypen ([**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) usw.), wie bereits beschrieben. Stattdessen wird untersucht, welche speziellen Eigenschaften Sie animieren, und es wird auf andere Faktoren wie die Vererbung und die Zusammensetzung der Steuerelemente geachtet. Es gibt Fälle, in denen eine Animation auch dann nur eine geringe Auswirkung auf den UI-Thread haben kann, wenn die UI von der Animation geändert wird. Sie kann stattdessen vom Kompositionsthread als unabhängige Animation behandelt werden.

Eine Animation ist unabhängig, wenn Sie diese Merkmale aufweist:

-   Das [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207)-Element der Animation hat einen Wert von 0Sekunden (siehe Warnhinweis).
-   Die Animation hat [**UIElement.Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962) als Ziel.
-   Die Animation hat einen Untereigenschaftswert dieser [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)-Eigenschaften: [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx), [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980), [**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection.aspx), [**Clip**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.clip).
-   Die Animation hat [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) oder [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772) als Ziel.
-   Die Animation hat einen [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)-Wert als Ziel und verwendet ein [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)-Element, für das [**Color**](https://msdn.microsoft.com/library/windows/apps/BR242963) animiert wird.
-   Die Animation ist ein [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)-Element.

**Achtung**  Damit Ihre Animation als unabhängige Animation behandelt wird, müssen Sie `Duration="0"` explizit festlegen. Wenn Sie z.B. `Duration="0"` aus diesem XAML-Beispiel entfernen, wird die Animation als abhängige Animation behandelt, obwohl die [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR243169) des Frames "0:0:0" ist.

 

```xml
<Storyboard>
    <DoubleAnimationUsingKeyFrames
         Duration="0"
         Storyboard.TargetName="Button2"
         Storyboard.TargetProperty="Width">
         <DiscreteDoubleKeyFrame KeyTime="0:0:0" Value="200" />
     </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

Wenn Ihre Animation diese Kriterien nicht erfüllt, handelt es sich wahrscheinlich um eine abhängige Animation. Vom Animationssystem wird eine abhängige Animation standardmäßig nicht ausgeführt. Es kann also sein, dass Sie die Ausführung der Animation während der Entwicklungs- und Testphase nicht beobachten können. Sie können die Animation trotzdem verwenden, aber Animationen dieser Art müssen einzeln aktiviert werden. Zum Aktivieren der Animation legen Sie die **EnableDependentAnimation**-Eigenschaft des Animationsobjekts auf **true** fest. (Jede [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517)-Unterklasse, die eine Animation darstellt, weist eine andere Implementierung der Eigenschaft auf, aber alle tragen den Namen `EnableDependentAnimation`.)

Die erforderliche Aktivierung abhängiger Animationen durch den App-Entwickler ist ein bewusst integrierter Entwurfsaspekt des Animationssystems und der Entwicklung. Damit soll Entwicklern bewusst gemacht werden, dass Animationen einen Leistungsaufwand bedeuten, der die Reaktionsfähigkeit der UI beeinträchtigen kann. In einer kompletten App lassen sich Animationen mit schlechter Leistung nur schwer isolieren und debuggen. Daher ist es besser, nur die abhängigen Animationen zu aktivieren, die Sie für die UI der App wirklich benötigen. Damit sollte es den Entwicklern nicht zu einfach gemacht werden, die Leistung von Apps aufgrund von dekorativen Animationen, für die viele Zyklen erforderlich sind, zu verschlechtern. Weitere Informationen zu Leistungstipps für Animationen finden Sie unter [Optimieren von Animationen und Medien](https://msdn.microsoft.com/library/windows/apps/Mt204774).

Als App-Entwickler können Sie sich auch für die Anwendung einer App-weiten Einstellung entscheiden, mit der abhängige Animationen immer deaktiviert werden – auch Animationen, für die **EnableDependentAnimation** auf **true** festgelegt ist. Informationen finden Sie unter [**Timeline.AllowDependentAnimations**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.allowdependentanimations).

**Tipp:**  Wenn Sie visuelle Zustände für ein Steuerelement mithilfe von Visual Studio erstellen, wird im Designer eine Warnung angezeigt, wenn Sie versuchen, eine abhängige Animation auf eine Eigenschaft für einen visuellen Zustand anzuwenden.

 

## Starten und Steuern einer Animation

Bisher haben wir Ihnen noch nicht gezeigt, wie eine Animation wirklich ausgeführt oder angewendet wird. Bis die Animation gestartet und ausgeführt wird, sind die Wertänderungen, die von einer Animation in XAML deklariert werden, lediglich latent vorhanden und werden noch nicht durchgeführt. Sie müssen eine Animation explizit auf eine Art und Weise starten, die sich auf die App-Lebensdauer oder die Benutzererfahrung bezieht. Auf der einfachsten Ebene starten Sie eine App, indem Sie die [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin)-Methode im [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) aufrufen, das als übergeordnetes Element der Animation fungiert. Sie können Methoden nicht direkt in XAML aufrufen. Jedwede Aktivierung von Animationen muss also über den Code durchgeführt werden. Dies ist entweder CodeBehind für die Seiten oder Komponenten der App oder die Logik des Steuerelements, wenn Sie eine benutzerdefinierte Steuerelementklasse definieren.

Normalerweise rufen Sie [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) auf und führen die Animation einfach bis zum Abschluss ihrer Zeitdauer aus. Sie können jedoch auch die Methoden [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx), [**Resume**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.resume.aspx) und [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop) verwenden, um das [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) zur Laufzeit zu steuern, sowie andere APIs, die für anspruchsvollere Animationssteuerungsszenarien verwendet werden.

Wenn Sie [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) für ein Storyboard mit Animationen aufrufen, die unendlich wiederholt werden (`RepeatBehavior="Forever"`), wird die Animation ausgeführt, bis die enthaltende Seite entladen wird oder bis Sie explizit [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx) oder [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop) aufrufen.

### Starten einer Animation aus dem App-Code

Sie können Animationen entweder automatisch oder als Reaktion auf Benutzeraktionen starten. Beim automatischen Starten nutzen Sie in der Regel ein Objektlebensdauerereignis wie [**Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723), das als Auslöser der Animation dient. Das **Loaded**-Ereignis ist hierfür gut geeignet, da die UI an diesem Punkt zur Interaktion bereit ist und die Animation am Anfang nicht abgeschnitten wird, weil noch ein anderer Teil der UI geladen werden muss.

In diesem Beispiel wird das [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed)-Ereignis an das Rechteck angefügt, sodass die Animation startet, wenn der Benutzer auf das Rechteck klickt.

```xml
<Rectangle PointerPressed="Rectangle_Tapped"
  x:Name="MyAnimatedRectangle"
  Width="300" Height="200" Fill="Blue"/>
  ```

Der Ereignishandler startet das [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) (die Animation), indem die [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin)-Methode des **Storyboard**-Elements verwendet wird.

> [!div class="tabbedCodeSnippets"]
``` csharp
myStoryboard.Begin();
```
``` cpp
myStoryboard->Begin();
```
``` vb
myStoryBoard.Begin()
```

Sie können das [**Completed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.completed.aspx)-Ereignis behandeln, wenn Sie andere Logik ausführen möchten, nachdem die Animation das Anwenden von Werten abgeschlossen hat. Für die Problembehandlung bei Eigenschaftssystem-/Animationsinteraktionen kann auch die [**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/BR242358)-Methode hilfreich sein.

**Tipp**  Wenn Sie den Code für ein App-Szenario erstellen, bei dem Sie eine Animation aus dem App-Code starten, sollten Sie noch einmal überprüfen, ob in der Animationsbibliothek für Ihr UI-Szenario nicht bereits eine Animation oder ein Übergang enthalten ist. Die Bibliotheksanimationen ermöglichen eine einheitlichere UI-Erfahrung in allen Windows-Runtime-Apps und sind einfacher zu verwenden.

 

### Animationen für visuelle Zustände

Das Ausführungsverhalten für ein [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490), das zum Definieren des visuellen Zustands eines Steuerelements verwendet wird, unterscheidet sich davon, wie eine App ein Storyboard ggf. direkt ausführt. Bei der Anwendung auf eine Definition des visuellen Zustands in XAML ist das **Storyboard** ein Element eines enthaltenden [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007)-Elements, und der gesamte Zustand wird mithilfe der [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager)-API gesteuert. Alle enthaltenen Animationen werden gemäß ihren Animationswerten und [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517)-Eigenschaften ausgeführt, wenn das enthaltende **VisualState**-Element von einem Steuerelement verwendet wird. Weitere Informationen finden Sie unter [Storyboardanimationen für visuelle Zustände](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808). Für visuelle Zustände unterscheidet sich das [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209)-Element. Wenn ein visueller Zustand in einen anderen Zustand geändert wird, werden alle auf den vorherigen visuellen Zustand angewendeten Eigenschaftsänderungen und die zugehörigen Animationen abgebrochen – auch dann, wenn der neue visuelle Zustand nicht ausdrücklich eine neue Animation auf eine Eigenschaft anwendet.

### **Storyboard** und **EventTrigger**

Es gibt eine Möglichkeit zum Starten einer Animation, bei der die Deklaration vollständig in XAML durchgeführt werden kann. Dieses Verfahren wird jedoch nicht mehr häufig angewendet. Es handelt sich um alte Syntax aus WPF und frühen Versionen von Silverlight, bevor die Unterstützung für [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager) hinzugefügt wurde. Diese [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390)-Syntax funktioniert aus Import- und Kompatibilitätsgründen in Windows-Runtime-XAML noch. Dies gilt jedoch nur für ein Auslöserverhalten basierend auf dem [**FrameworkElement.Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723)-Ereignis. Falls versucht wird, andere Ereignisse auszulösen, werden Ausnahmen ausgelöst, oder die Kompilierung schlägt fehl. Weitere Informationen finden Sie unter [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) oder [**BeginStoryboard**](https://msdn.microsoft.com/library/windows/apps/BR243053).

## Animieren von angefügten XAML-Eigenschaften

Auch wenn es sich nicht um ein gängiges Szenario handelt, können Sie einen animierten Wert auf eine angefügte XAML-Eigenschaft anwenden. Weitere Informationen zu angefügten Eigenschaften und ihrer Funktionsweise finden Sie unter [Übersicht über angefügte Eigenschaften](https://msdn.microsoft.com/library/windows/apps/Mt185579). Für die Zielbestimmung einer angefügten Eigenschaft benötigen Sie eine [Property-path-Syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586), bei der der Eigenschaftenname in Klammern gesetzt ist. Sie können die integrierten angefügten Eigenschaften wie beispielsweise [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/Hh759773) animieren, indem Sie ein [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)-Element verwenden, das diskrete Ganzzahlwerte anwendet. Allerdings sieht eine bestehende Einschränkung der XAML-Implementierung der Windows-Runtime vor, dass Sie eine benutzerdefinierte angefügte Eigenschaft nicht animieren können.

## Weitere Animationstypen und nächste Schritte zum Erlernen der UI-Animation

Bisher wurden die benutzerdefinierten Animationen vorgestellt, bei denen die Animation zwischen zwei Werten erfolgt und diese Werte während der Ausführung der Animation dann je nach Bedarf linear interpoliert werden. Diese Animationen werden als **From**/**To**/**By**-Animationen bezeichnet. Es gibt jedoch noch einen anderen Animationstyp, bei dem Sie Zwischenwerte deklarieren können, die zwischen dem Start- und Endwert liegen. Diese Animationen werden als *Keyframeanimationen* bezeichnet. Außerdem kann die Interpolationslogik für eine **From**/**To**/**By**-Animation oder eine Keyframeanimation geändert werden. Dazu muss eine Beschleunigungsfunktion angewendet werden. Weitere Informationen zu diesen Konzepten finden Sie unter [Keyframeanimationen und Animationen für Beschleunigungsfunktionen](key-frame-and-easing-function-animations.md).

## Verwandte Themen

* [Property-path-Syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [Übersicht über Abhängigkeitseigenschaften](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [Keyframeanimationen und Animationen für Beschleunigungsfunktionen](key-frame-and-easing-function-animations.md)
* [Storyboardanimationen für visuelle Zustände](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)
* [Steuerelementvorlagen](https://msdn.microsoft.com/library/windows/apps/Mt210948)
* [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824)
 

 







<!--HONumber=Aug16_HO3-->


