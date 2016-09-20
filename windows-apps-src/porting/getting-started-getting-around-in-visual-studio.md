---
author: mcleblanc
description: Aufbau von Visual Studio
title: Aufbau von Visual Studio
ms.assetid: 7FBB50A2-6D22-4082-B333-5153DADDDE9A
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d31e6e940f0b03667f1e19abec17804f6f3e16a6

---

# Erste Schritte: Aufbau von Visual Studio

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Aufbau von Microsoft Visual Studio

Kehren wir nun zum Projekt zurück, das wir zuvor erstellt haben. Dabei soll der Aufbau der integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) von Microsoft Visual Studio veranschaulicht werden.

Xcode-Entwickler sollten die im Folgenden dargestellte Standardansicht kennen. Dabei befinden sich die Quelldateien im linken Bereich und der Editor (für Benutzeroberfläche oder Quellcode) ist in der Mitte angeordnet. Im Bereich auf der rechten Seite sind die Steuerelemente und die zugehörigen Eigenschaften angeordnet.

![Xcode-Entwicklungsumgebung](images/ios-to-uwp/xcode-ide.png)

Microsoft Visual Studio ist ähnlich aufgebaut, obwohl sich die Steuerelemente in der Standardansicht auf der linken Seite in der **Toolbox** befinden. Die Quelldateien befinden sich im **Projektmappen-Explorer** auf der rechten Seite, und die Eigenschaften werden unter **Eigenschaften** im Bereich **Projektmappen-Explorer** wie folgt aufgeführt.

![Visual Studio-Entwicklungsumgebung](images/ios-to-uwp/vs-ide.png)

Wenn dies etwas ungewohnt für Sie ist, können Sie die Bereiche in Visual Studio neu anordnen und die Quelldateien im linken Bereich des Bildschirms und die Toolbox auf der rechten Seite platzieren. Sie können auf die Titelleiste eines beliebigen Bereichs klicken und diese ziehen, um sie neu anzuordnen. In Visual Studio wird anhand eines schattierten Felds angezeigt, an welcher Stelle sie nach Veröffentlichung angedockt wird. Viele Bereiche können auch ein kleines Reißzweckensymbol in der Titelleiste aufweisen. Damit können Sie den Bereich anheften und an dieser Stelle sperren. Beim lösen des Bereichs kann dieser ausgeblendet werden, um Platz zu sparen. Dies ist nützlich, wenn sich der Bildschirm auf der schmalen Seite befindet. Wählen Sie, wenn Sie sich vertan haben, **Fensterlayout zurücksetzen** im Menü **Fenster** aus, um die Reihenfolge wiederherzustellen.

## Hinzufügen von Steuerelementen, Festlegen der zugehörigen Eigenschaften und Reagieren auf Ereignisse

Lassen Sie uns dem Projekt nun einige Steuerelemente hinzufügen. Anschließend ändern wir einige der Steuerelementeigenschaften und schreiben Code, um auf eines der Steuerelementereignisse zu reagieren.

Zum Hinzufügen von Steuerelementen in Xcode öffnen Sie die gewünschte XIB-Datei oder das Storyboard, und fügen Sie dann per Drag & Drop wie im Folgenden dargestellt Steuerelemente hinzu, beispielsweise eine **rechteckige Schaltfläche mit abgerundeten Ecken** und eine **Bezeichnung**.

![Entwerfen der Benutzeroberfläche in Xcode](images/ios-to-uwp/xcode-add-button-label.png)

Lassen Sie uns nun einen ähnlichen Vorgang in Visual Studio ausführen. Ziehen Sie in der **Toolbox** das **Schaltflächen**-Steuerelement und legen Sie es auf der Entwurfsoberfläche der Datei „MainPage.xaml“ ab.

Wiederholen Sie den Vorgang für das **TextBlock**-Steuerelement, sodass es wie folgt aussieht:

![Entwerfen der Benutzeroberfläche in Visual Studio](images/ios-to-uwp/vs-add-button-label.png)

Im Gegensatz zu Xcode, wo Layout- und Bindungsinformationen in einer XIB- oder einer Storyboard-Datei gespeichert sind, wird in Visual Studio empfohlen, die zum Speichern dieser Details verwendeten XAML-Dateien in einer umfangreichen, bearbeitbaren, deklarativen XML-ähnlichen Sprache zu bearbeiten. Weitere Informationen zu Extensible Application Markup Language (XAML) finden Sie in der [XAML-Übersicht](https://msdn.microsoft.com/library/windows/apps/mt185595). Alle derzeit im Bereich **Entwurf** angezeigten Inhalte werden im **XAML**-Bereich definiert. Der **XAML**-Bereich ermöglicht bei Bedarf eine genaue Steuerung, und sobald Sie sich eingearbeitet haben, kommen Sie mit dem manuellen Schreiben von Benutzeroberflächencode schnell voran. An dieser Stelle konzentrieren wir uns jedoch ausschließlich auf die Bereiche **Entwurf** und **Eigenschaften**.

Ändern wir nun die Schaltflächendetails. Wie Sie nachvollziehen können, muss zum Ändern des Namens der Schaltfläche in Xcode der Wert des Felds **Titel** im Eigenschaftenpanel geändert werden.

Bei Verwendung von Visual Studio gehen Sie ähnlich vor. Tippen Sie im Bereich **Entwurf** auf die Schaltfläche, damit sie den Fokus erhält. Ändern Sie dann im Bereich **Eigenschaften** den Wert des Felds **Inhalt** von „Button“ in „Press Me“. Aktualisieren Sie als Nächstes den Namen des Schaltflächen-Steuerelements, indem Sie den **Name**-Wert von "&lt;No Name&gt;" in "myButton" ändern, wie hier gezeigt:

![Fenster "Schaltflächeneigenschaften" in Visual Studio](images/ios-to-uwp/vs-button-properties.png)

Lassen Sie uns nun Code zum Ändern der Inhalte des **TextBlock**-Steuerelements in „Hello, World!“ schreiben, sodass dieser Text angezeigt wird, nachdem der Benutzer auf die Schaltfläche getippt hat.

In Xcode würden Sie ein Ereignis zu einem Steuerelement zuordnen, indem Sie Code schreiben und diesen Code dann dem Steuerelement zuordnen. Dies geschieht häufig durch gesteuertes Ziehen der Schaltfläche in den Quellcode, wie im Folgenden dargestellt:

![Verknüpfen einer Schaltfläche mit einem Ereignis in Xcode](images/ios-to-uwp/xcode-add-button-event.png)

```swift
// Swift implementation.

@IBAction func buttonPressed(sender: UIButton) {
    
}
```

Visual Studio ist ähnlich aufgebaut. Rechts über den **Eigenschaften** befindet sich eine Schaltfläche mit einem Gewitterblitz. Hier werden dem ausgewählten Steuerelement zugeordnete mögliche Ereignisse wie folgt aufgeführt:

![Schaltflächenereignisliste in Visual Studio](images/ios-to-uwp/vs-button-event.png)

Um Code für das Click-Ereignis der Schaltfläche hinzuzufügen, wählen Sie im Bereich **Entwurf** zunächst die Schaltfläche aus. Klicken Sie als Nächstes auf die Schaltfläche mit dem Gewitterblitz, und doppelklicken Sie auf das leere Feld neben **Click**. Visual Studio fügt dann das Ereignis „myButton\_Click“ dem Feld **Click** hinzu. Anschließend wird der entsprechende Ereignishandler zur Datei „MainPage.xaml.cs“ hinzugefügt und angezeigt, wie im Folgenden dargestellt.

```csharp
private void myButton_Click(object sender, RoutedEventArgs e)
{

}
```

Jetzt verbinden wir das **TextBlock**-Steuerelement. In Xcode würden Sie die Schaltfläche bei gedrückter STRG-Taste in den Quellcode ziehen, um dem Steuerelement seine Definition zuzuordnen, wie hier gezeigt.

![Verknüpfen einer Bezeichnung mit der zugehörigen Definition in Xcode](images/ios-to-uwp/xcode-add-button-reference.png)

```swift
// Swift implentation.

@IBOutlet weak var myLabel : UILabel
```

In Visual Studio müssen Sie das Steuerelement nicht zuordnen, da dies immer automatisch erfolgt. Nun werden jedoch einige der Eigenschaften geändert:

1.  Tippen Sie auf die Registerkarte der Datei „MainPage.xaml“.
2.  Tippen Sie im Bereich **Entwurf** auf das **TextBlock**-Steuerelement.
3.  Tippen Sie im Bereich **Eigenschaften** auf die Schaltfläche mit dem Schraubenschlüssel, um die zugehörigen Eigenschaften anzuzeigen.
4.  Ändern Sie im Feld **Name** den Eintrag "&lt;No Name&gt;" in "myLabel".

![Fenster "Bezeichnungseigenschaften" in Visual Studio](images/ios-to-uwp/vs-label-properties.png)

Lassen Sie uns nun dem Click-Ereignis der Schaltfläche Code hinzufügen. Tippen Sie dazu auf die Datei „MainPage.xaml.cs“, und fügen Sie dem myButton\_Click-Ereignishandler den folgenden Code hinzu.

```csharp
private void myButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    myLabel.Text = "Hello, World!";
}
```

Dieser ist mit dem Code vergleichbar, den Sie in Swift schreiben:

```swift
@IBAction func buttonPressed(sender: UIButton) {
    myLabel.text = "Hello, World!"
}
```

Wählen Sie schließlich zum Ausführen der App das Menü **Debuggen** aus, und wählen Sie dann **Debuggen starten** (oder drücken Sie F5). Nachdem die App gestartet wurde, klicken Sie auf die Schaltfläche „Press Me“. Sie werden feststellen, dass der Inhalt der Bezeichnung von „TextBlock“ in „Hello, World!“ geändert wurde, wie in der folgenden Abbildung dargestellt.

![Ausführungsergebnisse der ersten exemplarischen Vorgehensweise: Hello, World!](images/ios-to-uwp/vs-hello-world.png)

Wenn Sie die App beenden möchten, tippen Sie in Visual Studio auf das Menü **Debuggen**, und tippen Sie dann auf **Debuggen beenden** (oder drücken Sie einfach UMSCHALT+F5). Beachten Sie, dass Sie in Visual Studio die App auf vielen unterschiedlichen Geräten testen können.

## Nächster Schritt

[Erste Schritte: Allgemeine Steuerelemente](getting-started-common-controls.md)




<!--HONumber=Jun16_HO4-->


