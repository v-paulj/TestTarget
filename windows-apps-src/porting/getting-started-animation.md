---
author: mcleblanc
title: Erste Schritte mit Animationen
ms.assetid: C1C3F5EA-B775-4700-9C45-695E78C16205
description: In diesem Projekt verschieben wir ein Rechteck, wenden einen Ausblendeeffekt an und blenden es wieder ein
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: 6e0b92af1d3c5f61aa2341d43ca40330fcc359f4

---

# Erste Schritte: Animationen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Hinzufügen von Animationen

In iOS werden Animationseffekte meist programmgesteuert erstellt. Sie könnten beispielsweise Animationen verwenden, die von den blockbasierten **animateWithDuration**-Methoden der **UIView**-Klasse oder den älteren, nicht blockbasierten Methoden bereitgestellt werden. Sie können auch explizit die **CALayer**-Klasse verwenden, um Ebenen zu animieren. Animationen in Windows-Apps können programmgesteuert erstellt werden. Sie können jedoch auch deklarativ mittels der Extensible Application Markup Language (XAML) definiert werden. Sie können Microsoft Visual Studio zum direkten Bearbeiten von XAML-Code verwenden oder auch das Visual Studio-Tool **Blend**, das XAML-Code beim Arbeiten mit Animationen in einem Designer erstellt. Mit Blend können Sie komplette VisualStudio-Projekte öffnen, entwerfen, erstellen und ausführen. In der folgenden exemplarischen Vorgehensweise können Sie diese Methode testen.

Erstellen Sie eine neue Universal Windows Platform (UWP)-App, und nennen Sie sie z.B. „SimpleAnimation“. In diesem Projekt verschieben wir ein Rechteck, wenden einen Ausblendeeffekt an und blenden es wieder ein In XAML basieren Animationen auf dem Konzept von *Storyboards* (nicht mit iOS-Storyboards zu verwechseln). Bei Storyboards werden Änderungen von Eigenschaften mithilfe von *Keyframes* animiert.

Klicken Sie mit der rechten Maustaste im **Projektmappen-Explorer** auf den Namen des Projekts, während das Projekt geöffnet ist, und wählen Sie **In Blend öffnen** oder **In Blend entwerfen** aus wie in der folgenden Abbildung gezeigt. Die Ausführung von Visual Studio wird im Hintergrund fortgesetzt.

![Menübefehl „In Blend öffnen“](images/ios-to-uwp/vs-open-in-blend.png)

Nachdem Blend gestartet wurde, sollte die Anzeige der folgenden Abbildung ähnlich sein.

![Blend-Entwicklungsumgebung](images/ios-to-uwp/blend-1.png)

Doppelklicken Sie auf der linken Seite auf **MainPage.xaml** im **Projektmappen-Explorer**. Wählen Sie als Nächstes auf der vertikalen Toolleiste am Rand der zentralen **Entwurfsansicht** das Tool **Rechteck** aus, und zeichnen Sie ein Rechteck in der **Entwurfsansicht** wie in der folgenden Abbildung dargestellt.

![Hinzufügen eines Rechtecks zur Entwurfsansicht](images/ios-to-uwp/blend-2.png)

Wenn Sie das Rechteck grün zeichnen möchten, klicken Sie im Fenster **Eigenschaften** im Bereich **Pinsel** auf die Schaltfläche **Pinsel mit Volltonfarbe**. Klicken Sie dann auf das Symbol **Farbformatpipette**. Klicken Sie auf eine beliebige Stelle im grünen Farbtonband.

Tippen Sie im Fenster **Objekte und Zeitachsen** auf das Plussymbol (**Neu**) und dann auf **OK**, um mit der Animation des Rechtecks zu beginnen, wie in der folgenden Abbildung gezeigt.

![Hinzufügen eines Storyboards](images/ios-to-uwp/blend-3.png)

Ein Storyboard wird im Fenster **Objekte und Zeitachsen** angezeigt (möglicherweise müssen Sie die Größe der Ansicht anpassen, damit es ordnungsgemäß angezeigt wird). Die Anzeige der **Designansicht** ändert sich, um anzuzeigen, dass die Zeitachsenaufzeichnung für Storyboard1 aktiviert ist****. Tippen Sie im Fenster **Objekte und Zeitachsen** über dem gelben Pfeil auf die Schaltfläche **Keyframe aufzeichnen**, um den aktuellen Status des Rechtecks zu erfassen, wie in der folgenden Abbildung gezeigt.

![Aufzeichnen eines Keyframes](images/ios-to-uwp/blend-4.png)

Lassen Sie uns das Rechteck nun verschieben und ausblenden. Ziehen Sie dazu den orangefarbenen/gelben Pfeil auf die 2-Sekunden-Position, und verschieben Sie dann das Rechteck leicht nach rechts. Ändern Sie dann wie in der folgenden Abbildung gezeigt im Fenster **Eigenschaften** im Bereich **Darstellung** die Eigenschaft **Undurchsichtigkeit** in den Wert **0**. Tippen Sie auf die Schaltfläche **Wiedergeben** im Storyboard-Bereich, um eine Vorschau der Animation anzuzeigen.

![Fenster „Eigenschaften“ und Schaltfläche „Wiedergeben“](images/ios-to-uwp/blend-5.png)

Als Nächstes möchten wir das Rechteck wieder einblenden. Doppelklicken Sie im Fenster **Objekte und Zeitachsen** auf **Storyboard1**. Wählen Sie dann wie in der folgenden Abbildung gezeigt im Fenster **Eigenschaften** im Bereich **Allgemein** **AutoReverse** aus.

![Auswählen eines Storyboards](images/ios-to-uwp/blend-6.png)

Klicken Sie schließlich auf die Schaltfläche **Wiedergeben**, um das Ergebnis zu überprüfen.

Sie können das Projekt erstellen und ausführen, indem Sie auf die grüne Schaltfläche „Ausführen“ am oberen Rand des Fensters klicken (oder F5 drücken). Wenn Sie dies tun, wird das Projekt tatsächlich erstellt und ausgeführt, das grüne Rechteck ist jedoch weiterhin vorhanden. Zum Starten der Animation müssen Sie dem Projekt eine Zeile mit Code hinzufügen. Gehen Sie dazu wie folgt vor.

Speichern Sie das Projekt, indem Sie das Menü **Datei** Menü öffnen und **MainPage.xaml speichern** wählen. Kehren Sie zu Visual Studio zurück. Wenn in Visual Studio ein Dialogfeld mit der Frage angezeigt wird, ob Sie die geänderte Datei erneut laden möchten, wählen Sie **Ja**. Doppelklicken Sie zum Öffnen auf die Datei **MainPage.xaml.cs** unter **MainPage.xaml**, und fügen Sie folgenden Code direkt über der public MainPage()-Methode hinzu:

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Add the following line of code.
    Storyboard1.Begin();
}
```

Führen Sie das Projekt erneut aus, und sehen Sie sich die Animation des Rechtecks an. Fertig!

Wenn Sie die Datei MainPage.xaml in der **XAML**-Ansicht öffnen, können Sie den XAML-Code sehen, den Blend für Sie hinzugefügt hat, während Sie im Designer gearbeitet haben. Sehen Sie sich insbesondere den Code in den `<Storyboard>`- und `<Rectangle>`-Elementen an. Der folgende Code zeigt ein Beispiel. Auslassungspunkte stellen Code ohne Bezug dar, der aus Platzgründen ausgelassen wird. Zur besseren Lesbarkeit des Codes wurden Zeilenumbrüche hinzugefügt.

```xml
...
<Storyboard 
        x:Name="Storyboard1" 
        AutoReverse="True">
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.RenderTransform).(CompositeTransform.TranslateX)"
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="0"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2" 
                Value="185.075"/>
    </DoubleAnimationUsingKeyFrames>
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.RenderTransform).(CompositeTransform.TranslateY)" 
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="0"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2" 
                Value="2.985"/>
    </DoubleAnimationUsingKeyFrames>
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.Opacity)" 
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="1"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2"
                Value="0"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
...
<Rectangle 
        x:Name="rectangle" 
        Fill="#FF00FF63" 
        HorizontalAlignment="Left" 
        Height="122" 
        Margin="151,312,0,0" 
        Stroke="Black" 
        VerticalAlignment="Top" 
        Width="239" 
        RenderTransformOrigin="0.5,0.5">
    <Rectangle.RenderTransform>
        <CompositeTransform/>
    </Rectangle.RenderTransform>
</Rectangle>
...
```

Sie können diesen XAML-Code manuell bearbeiten oder zu Blend zurückkehren, um dort weiter an diesem zu arbeiten. Mit Blend können Sie spielerisch interessante Benutzeroberflächen erstellen und sie mit einem Grafiktool animieren, was die Entwicklung erheblich beschleunigt. Weitere Informationen zu Animationen finden Sie unter [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/mt187350).


            **Hinweis**  Informationen zu Animationen für WindowsStore-Apps, die JavaScript und HTML verwenden, finden Sie unter [Animieren der Benutzeroberfläche (HTML)](https://msdn.microsoft.com/library/windows/apps/hh465165).

### Nächster Schritt

[Erste Schritte: Was kommt als Nächstes?](getting-started-what-next.md)



<!--HONumber=Jun16_HO4-->


