---
author: jwmsft
description: "In diesem Thema wird das Abhängigkeitseigenschaftensystem erläutert, das Ihnen beim Entwickeln einer Windows-Runtime-App mit C++, C# oder Visual Basic und XAML-Definitionen für die UI zur Verfügung steht."
title: "Übersicht über Abhängigkeitseigenschaften"
ms.assetid: AD649E66-F71C-4DAA-9994-617C886FDA7E
translationtype: Human Translation
ms.sourcegitcommit: 2791b5b80bf1405d3efdce5d81824dbe6d347b4f
ms.openlocfilehash: 5c61d4ff2f1efc6d4ce0ed292f2f856b23e53c91

---

# Übersicht über Abhängigkeitseigenschaften

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In diesem Thema wird das Abhängigkeitseigenschaftensystem erläutert, das Ihnen beim Entwickeln einer Windows-Runtime-App mit C++, C# oder Visual Basic und XAML-Definitionen für die UI zur Verfügung steht.

## Was ist eine Abhängigkeitseigenschaft?

Eine Abhängigkeitseigenschaft ist eine spezielle Art von Eigenschaft. Es handelt sich um eine Eigenschaft, bei der der Eigenschaftswert von einem dedizierten Eigenschaftensystem nachverfolgt und beeinflusst wird, das Teil der Windows-Runtime ist.

Damit eine Abhängigkeitseigenschaft unterstützt wird, muss das die Eigenschaft definierende Objekt ein [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) sein, d. h. eine Klasse, die irgendwo in der Vererbung die **DependencyObject**-Basisklasse enthält. Bei vielen Typen, die Sie bei einer Windows Store-App mit XAML für Ihre UI-Definitionen verwenden, handelt es sich um eine **DependencyObject**-Unterklasse mit Unterstützung von Abhängigkeitseigenschaften. Alle Typen, die aus einem Windows-Runtime-Namespace stammen, dessen Name nicht „XAML“ enthält, unterstützen jedoch keine Abhängigkeitseigenschaften. Eigenschaften dieses Typs sind gewöhnliche Eigenschaften, die nicht über das Abhängigkeitsverhalten des Eigenschaftensystems verfügen.

Der Zweck von Abhängigkeitseigenschaften besteht darin, es zu ermöglichen, den Wert einer Eigenschaft anhand anderer Eingaben systembedingt zu berechnen (andere Eigenschaften, Ereignisse und Zustände, die während der Ausführung der App eintreten). Zu diesen anderen Eingaben gehören:

-   Externe Eingabe wie Benutzereinstellung
-   Just-In-Time-Mechanismen zur Ermittlung von Eigenschaften, z. B. Datenbindung, Animationen und Storyboards
-   Vorlagen zur mehrfachen Verwendung, z. B. Ressourcen und Stile
-   Werte, die aufgrund von Beziehungen mit anderen Elementen in der Elementstruktur bekannt sind (übergeordnete und untergeordnete Elemente).

Eine Abhängigkeitseigenschaft repräsentiert oder unterstützt ein spezifisches Feature des Programmiermodells zum Definieren einer Windows-Runtime-App mit XAML für die UI und C#, Microsoft Visual Basic oder Visual C++-Komponentenerweiterungen (C++/CX) für den Code. Zu diesen Features gehören:

-   Datenbindung
-   Stile
-   Storyboardanimationen
-   „PropertyChanged“-Verhalten: Eine Abhängigkeitseigenschaft kann implementiert werden, um Callbacks bereitzustellen, die Änderungen an andere Abhängigkeitseigenschaften weitergeben können.
-   Verwenden eines Standardwerts, der aus Eigenschaftsmetadaten stammt
-   Allgemeines Dienstprogramm für Eigenschaftssystem, wie etwa [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) und die Metadatensuche

## Abhängigkeitseigenschaften und Windows-Runtime-Eigenschaften

Mit Abhängigkeitseigenschaften werden die grundlegenden Windows-Runtime-Eigenschaftsfunktionen erweitert, indem ein globaler, interner Eigenschaftenspeicher bereitgestellt wird, in dem alle Abhängigkeitseigenschaften einer App zur Laufzeit gesichert werden. Dies ist eine Alternative zur Standardvorgehensweise, bei der die Eigenschaft mit einem privaten Feld abgesichert wird, das in der Eigenschaftsdefinitionsklasse als privat festgelegt ist. Sie können sich diesen internen Eigenschaftenspeicher als einen Satz von Bezeichnern und Werten vorstellen, der für jedes einzelne Objekt vorhanden ist (sofern es sich um ein [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)-Element handelt). Die einzelnen Eigenschaften werden im Speicher nicht anhand des Namens identifiziert, sondern durch eine [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)-Instanz. Vom Eigenschaftensystem wird dieses Implementierungsdetail jedoch meist ausgeblendet: Sie können auf Abhängigkeitseigenschaften normalerweise über einen einfachen Namen (den programmgesteuerten Eigenschaftsnamen in der verwendeten Codesprache oder beim Schreiben von XAML-Code mithilfe eines Attributnamens) zugreifen.

Der Basistyp, der dem Abhängigkeitseigenschaftensystem zugrunde liegt, ist [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Mit **DependencyObject** werden Methoden definiert, die auf die Abhängigkeitseigenschaft zugreifen können. Instanzen einer abgeleiteten **DependencyObject**-Klasse stellen die interne Unterstützung des bereits erwähnten Eigenschaftenspeicherkonzepts bereit.

Im Folgenden finden Sie einen Überblick über die Terminologie, die für die Beschreibung der Abhängigkeitseigenschaften verwendet wird:

| Begriff | Beschreibung |
|------|-------------|
| Abhängigkeitseigenschaft | Eine Eigenschaft in einem [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)-Bezeichner (siehe unten). In der Regel ist dieser Bezeichner als statisches Element der definierenden abgeleiteten **DependencyObject**-Klasse verfügbar. |
| Abhängigkeitseigenschaftsbezeichner | Ein [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361), daher typischerweise öffentlich, obwohl schreibgeschützt. |
| Eigenschaftenwrapper | Die aufrufbaren **get**- und **set**-Implementierungen für eine Windows-Runtime-Eigenschaft. Oder die sprachspezifische Projektion der ursprünglichen Definition. Eine **get**-Eigenschaftenwrapper-Implementierung ruft [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) auf, wobei der jeweilige Abhängigkeitseigenschaftsbezeichner als die erste Eingabe und der Wert, der festgelegt werden soll, als zweite Eingabe übergeben werden. | 

Eigenschaftenwrapper bieten nicht nur einen bequemen Weg zum Aufrufen, sondern sie machen auch die Abhängigkeitseigenschaft für jeden Prozess, jedes Tool oder jede Projektion verfügbar, die Windows-Runtime-Definitionen für Eigenschaften verwenden.

Im folgenden Beispiel wird eine benutzerdefinierte„IsSpinning“-Abhängigkeitseigenschaft für C# definiert. Außerdem veranschaulicht das Beispiel die Beziehung zwischen dem Abhängigkeitseigenschaftsbezeichner und dem Eigenschaftenwrapper.

```csharp
// IsSpinningProperty is the dependency property identifier
// no need for info in the last PropertyMetadata parameter, so we pass null
public static readonly DependencyProperty IsSpinningProperty = 
    DependencyProperty.Register(
        "IsSpinning", typeof(Boolean),
        typeof(ExampleClass), null
    );
// The property wrapper, so that callers can use this property through a simple ExampleClassInstance.IsSpinning usage rather than requiring property system APIs
public bool IsSpinning
{
    get { return (bool)GetValue(IsSpinningProperty); }
    set { SetValue(IsSpinningProperty, value); }
}
```

**Hinweis**  Das Beispiel oben ist keine vollständige Anleitung zum Erstellen einer benutzerdefinierten Abhängigkeitseigenschaft. Damit sollen die Konzepte von Abhängigkeitseigenschaften für alle Personen verdeutlicht werden, die es vorziehen, Konzepte über den Code zu erlernen. Ein vollständigeres Beispiel finden Sie unter [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md).

## Rangfolge von Abhängigkeitseigenschaftswerten

Wenn Sie den Wert einer Abhängigkeitseigenschaft abrufen, erhalten Sie einen Wert, der für diese Eigenschaft mithilfe einer der Eingaben festgelegt wurde, die Teil des Windows-Runtime-Eigenschaftensystems sind. Die Rangfolge von Abhängigkeitseigenschaftswerten wird genutzt, damit vom Windows-Runtime-Eigenschaftensystem Werte auf vorhersehbare Weise berechnet werden können. Es ist auch wichtig, dass Sie mit der grundlegenden Reihenfolge vertraut sind, die für die Rangfolge gilt. Andernfalls kann es passieren, dass Sie eine Eigenschaft auf einer Rangfolgenebene festlegen möchten, während diese von einer anderen Komponente (System, Aufrufe von Dritten, Ihr eigener Code) auf einer anderen Ebene festgelegt wird. Sie sind dann frustriert, weil Sie herausfinden müssen, welcher Eigenschaftswert verwendet wird und woher dieser Wert stammt.

Zum Beispiel sind Stile und Vorlagen als gemeinsamer Ausgangspunkt zum Festlegen von Eigenschaftswerten und somit zum Festlegen der Darstellung eines Steuerelements vorgesehen. Nun kann es aber sein, dass Sie in einer bestimmten Steuerelementinstanz den Wert abweichend vom allgemeinen Vorlagenwert ändern möchten, beispielsweise wenn das Steuerelement eine andere Hintergrundfarbe oder einen anderen Text als Inhalt haben soll. Das Windows-Runtime-Eigenschaftssystem verwendet lokale Werte mit höherer Priorität als Werte, die von Stilen und Vorlagen bereitgestellt werden. Dadurch wird das Überschreiben der Vorlagen durch App-spezifische Werte ermöglicht, sodass die Steuerelemente nützlich für die eigene Verwendung in der App-UI sind.

### Rangfolgeliste für Abhängigkeitseigenschaften

Im Folgenden wird die maßgebliche Rangfolge beschrieben, die beim Zuweisen des Laufzeitwerts für eine Abhängigkeitseigenschaft im Eigenschaftensystem verwendet wird. Die höchste Priorität befindet sich an erster Stelle der Liste. Eine ausführlichere Erklärung folgt nach dieser Liste.

1.  **Animierte Werte:** Aktive Animationen, Animationen von Ansichtszuständen oder Animationen mit [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306)-Verhalten. Eine auf eine Eigenschaft angewendete Animation muss Vorrang vor dem Basiswert (nicht animiert) haben, auch wenn dieser Wert lokal festgelegt wurde, um die Wirksamkeit der Animation sicherzustellen.
2.  **Lokaler Wert:** Ein lokaler Wert kann ganz bequem über den Eigenschaftenwrapper festgelegt werden, was dem Festlegen eines Werts als Attribut oder Eigenschaftselement in XAML gleichkommt. Eine weitere Möglichkeit ist das Aufrufen der [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)-Methode über die Eigenschaft einer bestimmten Instanz. Wenn Sie einen lokalen Wert anhand einer Bindung oder einer statischen Ressource festlegen, werden die Bindung und die statische Ressource in der Rangfolge jeweils als festgelegter lokaler Wert behandelt. Somit werden Referenzen zu Bindungen oder Ressourcen gelöscht, wenn ein neuer lokaler Wert festgelegt wird.
3.  **Auf Vorlagen basierende Eigenschaften:** Ein Element verfügt über diese Eigenschaften, wenn es als Teil einer Vorlage erstellt wurde (über [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) oder [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)).
4.  **Style Setters (Stilsetter):** Werte aus einem [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)-Element innerhalb der Stile von Seiten- oder Anwendungsressourcen.
5.  **Standardwert:** Eine Abhängigkeitseigenschaft kann in ihren Metadaten einen Standardwert enthalten.

### Auf Vorlagen basierende Eigenschaften

Auf Vorlagen basierende Eigenschaften als Element in einer Rangfolgeliste werden nicht für jede Eigenschaft eines Elements angewendet, das Sie direkt in einem XAML-Seitenmarkup deklarieren. Das Konzept der auf Vorlagen basierenden Eigenschaft besteht nur für Objekte, die erstellt werden, wenn die Windows-Runtime eine XAML-Vorlage auf ein UI-Element anwendet und so dessen visuelle Effekte definiert.

Alle über eine Steuerelementvorlage festgelegte Eigenschaften besitzen Werte einer bestimmten Art. Diese Werte sind beinahe wie ein erweiterter Satz von Standardwerten für das Steuerelement. Sie sind häufig Werten zugeordnet, die Sie später durch direktes Festlegen der Eigenschaftswerte zurücksetzen können. Daher müssen die in der Vorlage festgelegten Werte von einem echten lokalen Wert unterschieden werden können, sodass ein neuer lokaler Wert ihn überschreiben kann.

**Hinweis**  
In einigen Fällen können von der Vorlage sogar lokale Werte überschrieben werden, falls mit der Vorlage keine Verweise auf die [{TemplateBinding}-Markuperweiterung](templatebinding-markup-extension.md) für Eigenschaften verfügbar gemacht werden konnten, die für Instanzen einstellbar sein sollten. Dies wird normalerweise nur durchgeführt, wenn die Eigenschaft nicht für die Festlegung für Instanzen bestimmt ist, sondern beispielsweise nur für visuelle Effekte und das Vorlagenverhalten relevant ist und nicht für die beabsichtigte Funktion oder Laufzeitlogik des Steuerelements, von dem die Vorlage verwendet wird.

###  Bindungen und Rangfolge

Bindungsvorgänge verfügen jeweils über die passende Rangfolge für den Bereich, für den sie verwendet werden sollen. So fungiert zum Beispiel eine Bindung, die auf einen lokalen Wert angewendet wird, als lokaler Wert, und eine Bindung ([{TemplateBinding}-Markuperweiterung](templatebinding-markup-extension.md)) für einen Eigenschaftssetter wird wie ein Stilsetter angewendet. Da Bindungen bis zur Laufzeit warten müssen, bis sie Werte aus Datenquellen abrufen können, wird der Vorgang zum Bestimmen der Eigenschaftswertrangfolge für jede Eigenschaft auch auf die Laufzeit ausgeweitet.

Bindungen haben nicht nur dieselbe Priorität wie lokale Werte, sondern sie sind auch lokale Werte. Dabei ist die Bindung ein Platzhalter für einen zurückgestellten Wert. Wenn für einen Eigenschaftswert eine Bindung eingerichtet ist und Sie dafür zur Laufzeit einen lokalen Wert festlegen, wird diese Bindung dadurch vollständig ersetzt. Wenn Sie [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) zum Definieren einer Bindung aufrufen, die erst zur Laufzeit aktiv wird, ersetzen Sie ebenfalls alle lokalen Werte, die Sie im XAML-Code oder in vorher ausgeführtem Code ggf. angewendet haben.

### Storyboardanimationen und Basiswert

Storyboardanimationen basieren auf dem Konzept des *Basiswerts*. Der Basiswert ist der Wert, der vom Eigenschaftensystem anhand seiner Rangfolge ermittelt wird, wobei der letzte Schritt zum Suchen nach Animationen jedoch wegfällt. Beispielsweise kann ein Basiswert aus der Vorlage eines Steuerelements oder aus der Festlegung eines lokalen Werts für die Instanz eines Steuerelements stammen. In beiden Fällen führt das Anwenden einer Animation dazu, dass dieser Basiswert überschrieben wird. Der animierte Wert wird so lange angewendet, wie die Animation ausgeführt wird.

Bei einer animierten Eigenschaft kann sich der Basiswert trotzdem auf das Verhalten der Animation auswirken, wenn die Animation sowohl **Von** als auch **Bis** nicht explizit angibt oder wenn die Animation die Eigenschaft auf ihren Basiswert zurücksetzt, nachdem sie abgeschlossen wurde. In diesen Fällen wird der Rest der Rangfolge wiederverwendet, nachdem die Ausführung einer Animation beendet ist.

Eine Animation, die **Von** mit einem [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306)-Verhalten festlegt, hat jedoch so lange Vorrang vor einem lokalen Wert, bis die Animation entfernt wird, obwohl die Animation augenscheinlich gestoppt wurde. Vom Konzept her entspricht dies einer Animation, die unendlich lange ausgeführt wird, auch wenn in der UI keine visuelle Animation vorhanden ist.

Mehrere Animationen können auf eine einzelne Eigenschaft angewendet werden. Jede dieser Animationen kann ggf. definiert worden sein, um Basiswerte zu ersetzen, die von verschiedenen Punkten der Wertrangfolge stammen. Diese Animationen werden zur Laufzeit jedoch alle gleichzeitig ausgeführt. Häufig bedeutet dies, dass ihre Werte kombiniert werden müssen, da jede Animation den gleichen Einfluss auf den Wert hat. Dies hängt davon ab, wie die Animationen genau definiert wurden, sowie vom animierten Werttyp.

Weitere Informationen finden Sie unter [Storyboardanimationen](https://msdn.microsoft.com/library/windows/apps/mt187354).

### Standardwerte

Das Erstellen eines Standardwerts für eine Abhängigkeitseigenschaft mit einem [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771)-Wert wird im Thema [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md) ausführlicher erläutert.

Abhängigkeitseigenschaften verfügen auch über Standardwerte, wenn diese Standardwerte in den Metadaten der Eigenschaft nicht explizit definiert wurden. Sofern sie von den Metadaten nicht geändert wurden, können die Standardwerte für die Windows-Runtime-Abhängigkeitseigenschaften in der Regel wie folgt lauten:

-   Eine Eigenschaft, die ein Laufzeitobjekt oder den grundlegenden **Object**-Typ (einen *Verweistyp*) verwendet, hat den Standardwert **null**. So ist beispielsweise [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) **null**, bis dafür ein anderer Wert festgelegt oder geerbt wird.
-   Eine Eigenschaft, die einen Basiswert verwendet, z. B. Zahlen oder einen booleschen Wert (einen *Werttyp*), nutzt für diesen Wert eine erwartete Standardangabe. Beispiel: 0 für ganze Zahlen und Gleitkommazahlen und **false** für einen booleschen Wert.
-   Eine Eigenschaft, die eine Windows-Runtime-Struktur verwendet, verfügt über einen Standardwert, der per Aufruf des impliziten Standardkonstruktors der Struktur abgerufen wird. Dieser Konstruktor nutzt die Standardangaben für die Basiswertfelder der Struktur. Eine Standardangabe für einen [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)-Wert wird beispielsweise mit den Werten **X** und **Y** als 0 initialisiert.
-   Eine Eigenschaft, die eine Enumeration verwendet, verfügt über einen Standardwert des ersten Members dieser Enumeration. In der Referenz können Sie die Standardwerte der einzelnen Enumerationen einsehen.
-   Eine Eigenschaft, die eine Zeichenfolge verwendet ([**System.String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx) für .NET, [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx) für C++/CX), verfügt als Standardwert über eine leere Zeichenfolge (**""**).
-   Sammlungseigenschaften werden in der Regel nicht als Abhängigkeitseigenschaften implementiert. Die Gründe hierfür werden weiter unten in diesem Thema erläutert. Wenn Sie jedoch eine benutzerdefinierte Sammlungseigenschaft implementieren und sie als Abhängigkeitseigenschaft verwenden möchten, vermeiden Sie unbedingt ein *unbeabsichtigtes Singleton*. Eine Beschreibung finden Sie am Ende von [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md).

## Von Abhängigkeitseigenschaften bereitgestellte Features

### Datenbindung

Für eine Abhängigkeitseigenschaft kann der Wert festgelegt werden, indem eine Datenbindung angewendet wird. Bei der Datenbindung wird im XAML-Code die Syntax unter [{Binding}-Markuperweiterung](binding-markup-extension.md) oder die [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Klasse im Code verwendet. Bei einer datengebundenen Eigenschaft wird die Ermittlung des endgültigen Eigenschaftswerts bis zur Laufzeit zurückgestellt. Der Wert wird dann aus einer Datenquelle abgerufen. Hierbei wird mithilfe des Abhängigkeitseigenschaftensystems ein Platzhalterverhalten für Vorgänge ermöglicht. Dies kann das Laden von XAML-Code sein, wenn der Wert noch nicht bekannt ist, und das anschließende Bereitstellen des Werts zur Laufzeit, indem eine Interaktion mit dem Datenbindungsmodul der Windows-Runtime erfolgt.

Im folgenden Beispiel wird der [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676)-Wert für ein [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element per Bindung im XAML-Code festgelegt. Bei der Datenbindung werden ein übernommener Datenkontext und eine Objektdatenquelle verwendet. (Letztere sind in dem verkürzten Beispiel nicht sichtbar. Ein umfassenderes Beispiel mit Kontext und Quelle finden Sie unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).)

```XML
<Canvas>
  <TextBlock Text="{Binding Team.TeamName}"/>
</Canvas>
```

Sie können Datenbindungen auch mit Code anstatt mit XAML erstellen. Informationen finden Sie unter [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257).

**Hinweis**  Bindungen dieser Art werden als lokaler Wert behandelt, um eine Rangfolge von Abhängigkeitseigenschaftswerten zu ermöglichen. Wenn Sie für eine Eigenschaft, für die ursprünglich ein [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Wert festgelegt wurde, einen anderen lokalen Wert festlegen, überschreiben Sie die Bindung damit vollständig, und nicht ihren Laufzeitwert.

### Bindungsquellen, Bindungsziele und die Rolle von FrameworkElement

Eine Eigenschaft muss nicht unbedingt eine Abhängigkeitseigenschaft sein, um als Bindungsquelle zu fungieren. In der Regel kann jede Eigenschaft eine Bindungsquelle sein, aber dies hängt von Ihrer Programmiersprache ab, für die jeweils bestimmte Grenzfälle gelten. Als Bindungsziel kommen jedoch ausschließlich Abhängigkeitseigenschaften in Frage.

Beachten Sie beim Erstellen einer Bindung im Code, dass die [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257)-API nur für [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) definiert wird. Sie können eine Bindungsdefinition stattdessen auch mit der Klasse [**BindingOperations**](https://msdn.microsoft.com/library/windows/apps/br209823) erstellen und somit jede [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)-Eigenschaft referenzieren.

Beachten Sie sowohl bei der Verwendung von Code als auch bei XAML, dass [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) eine [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)-Eigenschaft ist. Durch die Verwendung einer Art von über-/untergeordneter Eigenschaftsvererbung (meist im XAML-Markup) kann das Bindungssystem ein **DataContext**-Element auflösen, das zu einem übergeordneten Element gehört. Diese Vererbung kann auch ausgewertet werden, wenn das untergeordnete Objekt (das über die Zieleigenschaft verfügt) kein **FrameworkElement**-Element ist und daher keinen eigenen **DataContext**-Wert enthält. Das vererbte übergeordnete Element muss allerdings auf jeden Fall ein **FrameworkElement** sein, um die **DataContext**-Eigenschaft festlegen und annehmen zu können. Alternativ dazu müssen Sie die Bindung so definieren, dass sie auch mit einem **null**-Wert für **DataContext** funktioniert.

Das Verknüpfen der Bindung ist für die meisten Datenbindungsszenarien nicht die einzige Anforderung. Für eine effektive Bindung in eine oder zwei Richtungen muss die Quelleigenschaft Änderungsbenachrichtigungen unterstützen, die an das Bindungssystem und somit an das Ziel weitergegeben werden. Für benutzerdefinierte Bindungsquellen bedeutet dies, dass die Eigenschaft die Schnittstelle [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx) unterstützen muss. Sammlungen sollten die Schnittstelle [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) unterstützen. Bestimmte Klassen unterstützen diese Schnittstellen in ihren Implementierungen, sodass sie sich als Basisklasse für Datenbindungen anbieten. Ein Beispiel für eine solche Klasse ist [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx). Weitere Informationen zu Datenbindungen und zum Zusammenhang zwischen Datenbindungen und dem Eigenschaftensystem finden Sie unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

**Hinweis**  Von den hier aufgeführten Typen werden Microsoft .NET-Datenquellen unterstützt. C++/CX-Datenquellen nutzen andere Schnittstellen für Änderungsbenachrichtigungen oder feststellbares Verhalten. Weitere Informationen finden Sie unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

### Stile und Vorlagen

Stile und Vorlagen sind zwei Szenarien für Eigenschaften, die als Abhängigkeitseigenschaften definiert sind. Stile sind für das Festlegen von Eigenschaften geeignet, mit denen die UI der App definiert wird. Stile werden als Ressourcen in XAML festgelegt, und zwar entweder als Eintrag in einer [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740)-Sammlung oder in separaten XAML-Dateien, z. B. Designressourcenwörterbüchern. Stile interagieren mit dem Eigenschaftensystem, da sie sog. „Setter“ für Eigenschaften enthalten. Die wichtigste Eigenschaft hier ist die Eigenschaft [**Control.Template**](https://msdn.microsoft.com/library/windows/apps/br209465) einer Klasse [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390): Diese Eigenschaft definiert den größten Teil der visuellen Darstellung und des visuellen Zustands der Klasse **Control**. Weitere Informationen zu Stilen und XAML-Beispiele, in denen [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) definiert und Setter verwendet werden, finden Sie unter [Formatieren von Steuerelementen](https://msdn.microsoft.com/library/windows/apps/mt210950).

Werte, die aus Stilen oder Vorlagen stammen, sind zurückgestellte Werte (ähnlich wie bei Bindungen). So können Benutzer von Steuerelementen neue Vorlagen für Steuerelemente erstellen oder Stile neu definieren. Aus diesem Grund können Eigenschaftensetter in Stilen nur für Abhängigkeitseigenschaften und nicht für normale Eigenschaften verwendet werden.

### Storyboardanimationen

Sie können den Wert einer Abhängigkeitseigenschaft mithilfe von Storyboardanimationen animieren. Storyboardanimationen in der Windows-Runtime dienen nicht nur reinen Dekorationszwecken. Stellen Sie sich Animationen eher als Zustandsautomatverfahren vor, mit dem die Werte einzelner Eigenschaften oder aller Eigenschaften und visuellen Elemente eines Steuerelements festgelegt und im Laufe der Zeit geändert werden können.

Die Zieleigenschaft einer Animation muss eine Abhängigkeitseigenschaft sein, um animiert werden zu können. Zudem muss der Wert einer Zieleigenschaft von einem der von der vorhandenen Klasse [**Timeline**](https://msdn.microsoft.com/library/windows/apps/br210517) abgeleiteten Animationstypen unterstützt werden. Werte von [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723), [**Double**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) und [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) können entweder mit Interpolations- oder Keyframeverfahren animiert werden. Die meisten anderen Werte können mithilfe von diskreten **Object**-Keyframes animiert werden.

Wenn eine Animation angewendet wurde und ausgeführt wird, verfügt der animierte Wert über eine höhere Priorität in der Rangfolge als alle anderen Werte (z. B. ein lokaler Wert), die der Eigenschaft sonst noch zugeordnet sind. Animationen können optional ein [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306)-Verhalten aufweisen, das dazu führen kann, dass Animationen Eigenschaftswerte anwenden, obwohl die Animation augenscheinlich gestoppt wurde.

Das Prinzip des Zustandsautomaten liegt der Verwendung von Storyboardanimationen als Teil des [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021)-Zustandsmodells für Steuerelemente zugrunde. Weitere Informationen zu Storyboardanimationen finden Sie unter [Storyboardanimationen](https://msdn.microsoft.com/library/windows/apps/mt187354). Weitere Informationen zu **VisualStateManager** und zur Definition von Ansichtszuständen für Steuerelemente finden Sie unter [Storyboardanimationen für visuelle Zustände](https://msdn.microsoft.com/library/windows/apps/xaml/jj819808) oder [Steuerelementvorlagen](../controls-and-patterns/control-templates.md).

### Verhalten bei „PropertyChanged“-Ereignis

Aufgrund des Verhaltens bei einem „PropertyChanged“-Ereignis wird eine Abhängigkeitseigenschaft überhaupt erst Abhängigkeitseigenschaft genannt. Die Erhaltung der Gültigkeit von Eigenschaftswerten in Konstellationen, in denen andere Eigenschaften den Wert der ersten Eigenschaft beeinflussen können, ist bei vielen Frameworks eine komplexe Herausforderung bei der Entwicklung. Im Windows-Runtime-Eigenschaftensystem kann jede Abhängigkeitseigenschaft ein Callback festlegen, das aufgerufen wird, sobald sich der Eigenschaftswert ändert. Mit diesem Callback können zugehörige Eigenschaften in der Regel synchron benachrichtigt oder deren Werte geändert werden. Viele bestehende Abhängigkeitseigenschaften haben ein Verhalten bei einem "PropertyChanged"-Ereignis. Sie können auch ein ähnliches Callback-Verhalten für Abhängigkeitseigenschaften festlegen und Ihre eigenen PropertyChanged-Callbacks implementieren. Unter [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md) finden Sie ein entsprechendes Beispiel.

### Standardwert und **ClearValue**

Für eine Abhängigkeitseigenschaft kann ein Standardwert als Teil der Metadaten der jeweiligen Eigenschaft definiert werden. Für eine Abhängigkeitseigenschaft wird ihr Standardwert nicht irrelevant, nachdem die Eigenschaft zum ersten Mal festgelegt wurde. Der Standardwert gilt zur Laufzeit möglicherweise jeweils erneut, wenn eine andere Determinante in der Wertrangfolge entfernt wird. (Der Vorrang von Abhängigkeitseigenschaftenwerten wird im nächsten Abschnitt erläutert). Zum Beispiel entfernen Sie unter Umständen zwar bewusst einen Formatwert oder eine Animation, der/die für eine Eigenschaft gilt, möchten aber, dass der Wert weiterhin ein angemessener Standardwert ist. Der Standardwert der Abhängigkeitseigenschaft kann diesen Wert bereitstellen, ohne dass die Werte der einzelnen Eigenschaften zusätzlich festgelegt werden müssen.

Sie können eine Eigenschaft auch dann absichtlich auf den Standardwert festlegen, wenn die Eigenschaft bereits einem lokalen Wert zugewiesen wurde. Rufen Sie die [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357)-Methode auf, um einen Wert auf den Standardwert zurückzusetzen und andere Teilnehmer in der Rangfolge zu aktivieren, die den Standardwert, nicht aber einen lokalen Wert außer Kraft setzen können (verweisen Sie auf die zu löschende Eigenschaft als Methodenparameter). Sie möchten nicht in allen Fällen, dass die Eigenschaft genau den Standardwert verwendet. Das Löschen des lokalen Werts und das Zurücksetzen auf den Standardwert kann jedoch zur Aktivierung eines anderen Elements in der Rangfolge führen, das sofort behandelt werden soll, z. B. bei der Verwendung des Werts aus einem Style Setter in einer Steuerelementvorlage.

## **DependencyObject** und Threading

Alle [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)-Instanzen müssen in dem Benutzeroberflächenthread erstellt werden, der mit dem aktuellen [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) verknüpft ist, das von einer Windows-Runtime-App angezeigt wird. Obwohl jede **DependencyObject** im zentralen UI-Thread erstellt werden muss, kann durch den Zugriff auf die [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616)-Eigenschaft mithilfe eines Verteilerverweises von anderen Threads aus auf die Objekte zugegriffen werden. Sie können anschließend Methoden wie [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) für das [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)-Objekt aufrufen und Ihren Code gemäß den Regeln der Threadeinschränkungen im UI-Thread ausführen.

Die Threadingmerkmale von [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) sind relevant, da dies in der Regel bedeutet, dass nur im Benutzeroberflächenthread ausgeführter Code den Wert einer Abhängigkeitseigenschaft ändern oder auch nur lesen kann. Threadingprobleme können in typischem Benutzeroberflächencode in der Regel vermieden werden, wenn **async**-Muster und Hintergrundworkerthreads richtig verwendet werden. Threadingprobleme im Zusammenhang mit **DependencyObject** entstehen in der Regel nur, wenn Sie eigene **DependencyObject**-Typen definieren und versuchen, diese als Datenquellen oder für andere Szenarien zu verwenden, für die ein **DependencyObject** nicht notwendigerweise geeignet ist.

## Verwandte Themen

**Konzeptionelles Material**
* [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md)
* [Übersicht über angefügte Eigenschaften](attached-properties-overview.md)
* [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [Storyboardanimationen](https://msdn.microsoft.com/library/windows/apps/mt187354)
* [Erstellen von Komponenten für Windows-Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [XAML-Beispiel für Benutzer und benutzerdefinierte Steuerelemente](http://go.microsoft.com/fwlink/p/?linkid=238581)
            
          
            **APIs für Abhängigkeitseigenschaften**
* [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
* [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)




<!--HONumber=Jun16_HO4-->


