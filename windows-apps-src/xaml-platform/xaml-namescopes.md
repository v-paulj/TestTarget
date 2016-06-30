---
author: jwmsft
description: Ein XAML-NameScope speichert Beziehungen zwischen den definierten XAML-Namen von Objekten und ihren entsprechenden Instanzen. Dies ist vergleichbar mit der weiteren Bedeutung des Begriffs NameScope in anderen Programmiersprachen und Technologien.
title: XAML-Namescopes
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: b8b833f40bc38799acc8813d38ddea63426f05b3

---

# XAML-Namescopes

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Ein *XAML-NameScope* speichert Beziehungen zwischen den definierten XAML-Namen von Objekten und ihren entsprechenden Instanzen. Dies ist vergleichbar mit der weiteren Bedeutung des Begriffs *NameScope* in anderen Programmiersprachen und Technologien.

## Definieren von XAML-NameScopes

Die Namen in XAML-NameScopes aktivieren Benutzercode, der auf die Objekte verweist, die ursprünglich in XAML definiert wurden. Das interne Ergebnis der XAML-Analyse ist das Erstellen eines Satzes von Objekten, in dem einige oder alle Beziehungen dieser Objekte in den XAML-Deklarationen beibehalten werden. Diese Beziehungen werden als spezifische Objekteigenschaften der erstellten Objekte beibehalten oder für Hilfsmethoden in den Programmiermodell-APIs verfügbar gemacht.

Die typischste Verwendung eines Namens in einem XAML-NameScope ist der direkte Verweis auf eine Objektinstanz, die vom Markupkompilierungsschritt aktiviert wird, in Kombination mit einer generierten **InitializeComponent**-Methode in den partiellen Klassenvorlagen.

Sie können auch die [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)-Hilfsmethode zur Laufzeit verwenden, um einen Verweis auf Objekte zurückzugeben, die mit einem Namen im XAML-Markup definiert wurden.

### Mehr zu Buildaktionen und XAML

Technisch gesehen wird für das XAML selbst ein Markupcompilerdurchlauf durchgeführt, während gleichzeitig das XAML und die darin für CodeBehind definierte partielle Klasse gemeinsam kompiliert werden. Jedes Objektelement, für das im Markup ein **Name**- oder [x:Name-Attribut](x-name-attribute.md) definiert wurde, generiert ein internes Feld mit einem Namen, der mit dem XAML-Namen übereinstimmt. Dieses Feld ist anfänglich leer. Die Klasse generiert dann eine **InitializeComponent**-Methode, die erst aufgerufen wird, wenn das gesamte XAML geladen wurde. Anschließend werden durch die **InitializeComponent**-Logik alle internen Felder mit dem [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)-Rückgabewert für die entsprechende Namenszeichenfolge aufgefüllt. Sie können diese Infrastruktur selbst erkennen, indem Sie sich die Dateien mit der Erweiterung ".g" (generiert) ansehen, die für die einzelnen XAML-Seiten nach der Kompilierung im /obj-Unterordner eines Projekts für eine Windows-Runtime-App erstellt werden. Sie können die Felder und die **InitializeComponent**-Methode auch als Member der erstellten Assemblys anzeigen, wenn Sie eine Reflexion über diese ausführen oder in anderer Weise ihre MSIL-Inhalte untersuchen.

**Hinweis**  Speziell bei Visual C++-Komponentenerweiterungen (C++/CX) wird für das Stammelement einer XAML-Datei kein Sicherungsfeld für einen **x:Name**-Verweis erstellt. Wenn Sie aus CodeBehind für C++/CX auf das Stammobjekt verweisen müssen, verwenden Sie andere APIs oder eine Strukturausnahme. Sie können beispielsweise erst [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) für ein bekanntes benanntes Unterelement und anschließend [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739) aufrufen.

## Erstellen von Objekten zur Laufzeit mit XamlReader.Load

XAML kann auch als Zeichenfolgeneingabe für die [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)-Methode verwendet werden, die analog zum ursprünglichen XAML-Quellanalysevorgang funktioniert. **XamlReader.Load** erstellt zur Laufzeit eine neue getrennte Struktur von Objekten. Die getrennte Struktur kann dann an die Hauptobjektstruktur angefügt werden. Sie müssen die erstellte Objektstruktur explizit verbinden, entweder durch Hinzufügen zu einer Inhaltseigenschaftenauflistung wie **Children** oder durch Festlegen einer anderen Eigenschaft, die einen Objektwert akzeptiert (z. B. Laden eines neuen [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) für einen [**Fill**](https://msdn.microsoft.com/library/windows/apps/br243378)-Eigenschaftswert).

### XAML-NameScope-Auswirkungen von XamlReader.Load

Der vorläufige XAML-NameScope, der durch die neue mit [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) erstellte Objektstruktur definiert wird, wertet alle definierten Namen im bereitgestellten XAML auf Eindeutigkeit hin aus. Wenn das bereitgestellte XAML zu diesem Zeitpunkt intern nicht eindeutige Namen enthält, wird durch **XamlReader.Load** eine Ausnahme ausgelöst. Beim Verbinden der getrennten Objektstruktur mit der Hauptobjektstruktur der Anwendung wird nicht versucht, den zugehörigen XAML-NameScope mit dem XAML-Haupt-NameScope der Anwendung zusammenzuführen. Nach dem Verbinden der Strukturen weist die App eine einheitliche Objektstruktur auf, die jedoch einzelne XAML-NameScopes enthält. Die Trennungen treten an den Verbindungspunkten zwischen Objekten auf, an denen Sie Eigenschaften auf den Wert festlegen, der von einem **XamlReader.Load**-Aufruf zurückgegeben wird.

Das Problem mit einzelnen und getrennten XAML-NameScopes besteht darin, dass Aufrufe der [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)-Methode sowie direkte verwaltete Objektverweise nicht mehr für einen einheitlichen XAML-NameScope durchgeführt werden können. Stattdessen impliziert das bestimmte Objekt, für das **FindName** aufgerufen wird, den Bereich. Dabei ist der Bereich der XAML-NameScope, in dem sich das aufrufende Objekt befindet. Bei direkten verwalteten Objektverweisen wird der Bereich durch die Klasse impliziert, die den Code enthält. In der Regel ist der CodeBehind für Laufzeitinteraktionen einer "Seite" mit App-Inhalt in der partiellen Klasse vorhanden, auf der die "Seite" für den Stamm basiert. Aus diesem Grund fungiert der XAML-NameScope XAML-Stamm-NameScope.

Wenn Sie [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) aufrufen, um ein benanntes Objekt aus dem XAML-Stamm-NameScope abzurufen, findet die Methode keine Objekte aus einem mit [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) erstellten einzelnen XAML-NameScope. Umgekehrt findet die Methode keine benannten Objekte im XAML-Stamm-NameScope, wenn Sie **FindName** von einem Objekt aus aufrufen, das sich in einem einzelnen XAML-NameScope befindet.

Dieses Problem mit einzelnen XAML-NameScopes wirkt sich nur auf Objektsuchen nach Namen in XAML-NameScopes aus, die mithilfe des [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)-Aufrufs durchgeführt werden.

Um Verweise auf Objekte abzurufen, die in einem anderen XAML-NameScope definiert werden, stehen mehrere Vorgehensweisen zur Verfügung:

-   Gehen Sie in einzelnen Schritten durch die ganze Struktur mit [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739) und/oder Auflistungseigenschaften, die bekanntermaßen in der Objektstruktur vorhanden sind (z. B. die von [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514) zurückgegebene Auflistung).
-   Wenn Sie den Aufruf von einem einzelnen XAML-NameScope aus durchführen und auf den XAML-Stamm-NameScope abzielen, bietet es sich an, einen Verweis auf das gegenwärtig angezeigte Hauptfenster abzurufen. Sie können den visuellen Stamm (das XAML-Stammelement, das auch als Inhaltsquelle bezeichnet wird) aus der aktuellen Anwendung in einer Codezeile mit dem `Window.Current.Content`-Aufruf abrufen. Sie können dann eine Umwandlung in [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) durchführen und von diesem Bereich aus [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) aufrufen.
-   Wenn Sie den Aufruf vom XAML-Stamm-NameScope aus ausführen und auf ein Objekt in einem einzelnen XAML-NameScope abzielen, bietet es sich an, in Ihrem Code vorauszuplanen und einen Verweis auf das Objekt beizubehalten, das von [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) und anschließend der Hauptobjektstruktur hinzugefügt wurde. Dieses Objekt ist jetzt ein gültiges Objekt zum Aufrufen von [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) innerhalb des einzelnen XAML-NameScopes. Speichern Sie dieses Objekt als globale Variable, oder übergeben Sie es mit Methodenparametern.
-   Sie können Probleme hinsichtlich Namen und XAML-NameScope vermeiden, indem Sie die visuelle Struktur untersuchen. Die [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/br243038)-API ermöglicht es Ihnen, die visuelle Struktur im Hinblick auf übergeordnete Objekte und untergeordnete Auflistungen auf Grundlage von Position und Index zu durchlaufen.

## XAML-NameScopes in Vorlagen

Mithilfe von Vorlagen in XAML können Sie Inhalt auf einfache Weise wiederverwenden und erneut anwenden. Vorlagen können jedoch auch Elemente mit Namen enthalten, die auf Vorlagenebene definiert wurden. Dieselbe Vorlage kann in einer Seite mehrmals verwendet werden. Deshalb definieren Vorlagen ihre eigenen XAML-NameScopes, und zwar unabhängig von der Seite, auf die der Stil oder die Vorlage angewendet wird. Betrachten Sie das folgende Beispiel:

```xml
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  >
  <Page.Resources>
    <ControlTemplate x:Key="MyTemplate">
      ....
      <TextBlock x:Name="MyTextBlock" />
    </ControlTemplate>
  </Page.Resources>
  <StackPanel>
    <SomeControl Template="{StaticResource MyTemplate}" />
    <SomeControl Template="{StaticResource MyTemplate}" />
  </StackPanel>
</Page>
```

Hier wird dieselbe Vorlage auf zwei verschiedene Steuerelemente angewendet. Wenn die Vorlagen keine getrennten XAML-NameScopes enthalten würden, würde der in der Vorlage verwendete MyTextBlock-Name einen Namenskonflikt auslösen. Jede Instanziierung der Vorlage verfügt über einen eigenen XAML-NameScope, sodass in diesem Beispiel der XAML-NameScope jeder instanziierten Vorlage genau einen Namen enthält. Der XAML-Stamm-NameScope enthält jedoch keinen der Vorlagennamen.

Wegen der separaten XAML-NameScopes erfordert die Suche nach benannten Elementen innerhalb einer Vorlage vom Bereich der Seite, auf die die Vorlage angewendet wird, ein anderes Verfahren. Statt [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) für ein Objekt in der Objektstruktur aufzurufen, rufen Sie zunächst das Objekt ab, auf das die Vorlage angewendet wurde. Rufen Sie dann [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) auf. Wenn Sie Autor eines Steuerelements sind und eine Konvention erstellen, in der ein bestimmtes benanntes Element in einer angewendeten Vorlage das Ziel für ein Verhalten darstellt, das vom Steuerelement selbst definiert wird, können Sie die **GetTemplateChild**-Methode aus dem Code zur Implementierung des Steuerelements verwenden. Die **GetTemplateChild**-Methode ist geschützt, sodass nur der Steuerelementautor Zugriff darauf hat. Außerdem gibt es Konventionen, die Steuerelementautoren befolgen sollten, um Teile und Vorlagenteile zu benennen und diese als auf die Steuerelementklasse angewendete Attributwerte zu melden. Diese Vorgehensweise macht die Namen wichtiger Teile für die Benutzer von Steuerelementen erkennbar, die möglicherweise eine andere Vorlage anwenden möchten, die die benannten Teile ersetzen müsste, um die Funktionalität der Steuerelemente aufrechtzuerhalten.

## Verwandte Themen

* [Übersicht über XAML](xaml-overview.md)
* [x:Name-Attribut](x-name-attribute.md)
* [Schnellstart: Steuerelementvorlagen](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)
* [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)
 




<!--HONumber=Jun16_HO4-->


