---
author: jwmsft
description: Im Folgenden werden die XML/XAML-Namespacezuordnungen (xmlns) erläutert, wie sie im Stammelement der meisten XAML-Dateien zu finden sind. Darüber hinaus wird erläutert, wie ähnliche Zuordnungen für benutzerdefinierte Typen und Assemblys erstellt werden.
title: XAML-Namespaces und Namespacezuordnung
ms.assetid: A19DFF78-E692-47AE-8221-AB5EA9470E8B
---

# XAML-Namespaces und Namespacezuordnung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Im Folgenden werden die XML/XAML-Namespacezuordnungen (**xmlns**) erläutert, wie sie im Stammelement der meisten XAML-Dateien zu finden sind. Darüber hinaus wird erläutert, wie ähnliche Zuordnungen für benutzerdefinierte Typen und Assemblys erstellt werden.

## Bezug von XAML-Namespaces zu Codedefinitions- und Typbibliotheken

Die XAML-Sprache wird sowohl im Allgemeinen als auch bei der Programmierung von Windows-Runtime-Apps dazu verwendet, Objekte, Eigenschaften dieser Objekte und in Hierarchien dargestellte Objekt/Eigenschaft-Beziehungen zu deklarieren. Die in XAML deklarierten Objekte werden von Typbibliotheken oder anderen Darstellungen unterstützt, die von anderen Programmiermethoden und -sprachen definiert werden. Bei diesen Bibliotheken kann es sich um Folgende handeln:

-   Den integrierten Objektsatz für die Windows-Runtime. Dabei handelt es sich um einen festen Satz mit Objekten, und beim Zugriff auf diese Objekte über XAML wird eine interne Logik zur Typzuordnung und Aktivierung verwendet.
-   Verteilte Bibliotheken, die entweder von Microsoft oder von Dritten zur Verfügung gestellt werden.
-   Bibliotheken, die die Definition eines Drittanbieter-Steuerelements darstellen und die in Ihrer App und Ihren weitervertreibbaren Paketkomponenten integriert sind.
-   Ihre eigene Bibliothek, die zu Ihrem Projekt gehört und die alle Benutzercodedefinitionen oder einen Teil davon enthält.

Unterstützungsinformationen sind bestimmten XAML-Namespacedefinitionen zugeordnet. XAML-Frameworks, wie die Windows-Runtime, können mehrere Assemblys und Code-Namespaces aggregieren, die einem einzelnen XAML-Namespace zugeordnet werden. Damit wird das Konzept eines XAML-Vokabulars möglich, das ein größeres Programmierframework oder eine größere Programmiertechnologie abdeckt. Ein XAML-Vokabular kann ziemlich umfangreich sein, beispielsweise stellt ein Großteil des für Windows-Runtime-Apps dokumentierten XAML-Codes ein einziges XAML-Vokabular dar. Ein XAML-Vokabular ist auch erweiterbar: Sie erweitern es, indem Sie den zugrunde liegenden Codedefinitionen Typen hinzufügen. Dabei muss darauf geachtet werden, dass die Typen in Codenamespaces aufgenommen werden, die bereits als zugeordnete Namespacequellen für das XAML-Vokabular verwenden werden.

Ein XAML-Verarbeiter kann Typen und Elemente in den Unterstützungsassemblys suchen, die dem XAML-Namespace zugeordnet sind, wenn er eine Laufzeitobjektdarstellung erstellt. Aus diesem Grund ist XAML als Methode zur Formalisierung und zum Austausch von Definitionen für das Objektkonstruktionsverhalten sehr nützlich, und daher wird XAML auch als Benutzeroberflächen-Definitionsmethode für Windows Store-Apps verwendet.

## XAML-Namespaces bei typischer XAML-Markupverwendung

Eine XAML-Datei deklariert beinahe immer einen Standard-XAML-Namespace im Stammelement. Der Standard-XAML-Namespace definiert, welche Elemente deklariert werden können, ohne sie durch ein Präfix zu qualifizieren. Wenn ein Element zum Beispiel als `<Balloon />` deklariert wird, erwartet ein XAML-Parser, dass das Element **Balloon** existiert und im Standard-XAML-Namespace gültig ist. Dagegen müssen Sie den Elementnamen mit einem Präfix qualifizieren, wenn **Balloon** nicht der definierte Standard-XAML-Namespace ist, etwa `<party:Balloon />`. Das Präfix gibt an, dass das Element bereits in einem anderen XAML-Namespace als dem Standardnamespace existiert, und Sie müssen dem Präfix **party** einen anderen XAML-Namespace zuordnen, bevor Sie dieses Element verwenden können. XAML-Namespaces beziehen sich speziell auf das Element, für das sie deklariert wurden, und darüber hinaus auf alle Elemente, die sich in diesem Element in der XAML-Struktur befinden. Aus diesem Grund werden XAML-Namespaces so gut wie immer für Stammelemente einer XAML-Datei deklariert, sodass die Vorteile dieser Vererbung genutzt werden können.

## Die standardmäßigen XAML-Sprach-XAML-Namespace-Deklarationen

Innerhalb des Stammelements der meisten XAML-Dateien sind zwei **xmlns**-Deklarationen zu finden. Die erste Deklaration ordnet einen XAML-Namespace als Standard zu: `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`

Dabei handelt es sich um denselben XAML-Namespacebezeichner, der bereits in vielen Vorgängertechnologien von Microsoft zum Einsatz kam, die ebenfalls XAML als Markupformat für die Oberflächendefinition genutzt haben. Die Verwendung desselben Bezeichners erfolgt bewusst und ist nützlich, wenn Sie bereits zuvor definierte Benutzeroberflächen mithilfe von C++, C# oder Visual Basic in eine Windows-Runtime-App migrieren.

Die zweite Deklaration ordnet für die XAML-definierten Sprachelemente einen separaten XAML-Namespace zu, wobei die Zuordnung normalerweise zum Präfix „x:“ erfolgt: `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`

Dieser **xmlns**-Wert und das Präfix „x:“, dem er zugeordnet ist, sind ebenfalls identisch mit den Definitionen, die in verschiedenen Vorgängertechnologien mit XAML von Microsoft verwendet wurden.

Die Beziehung zwischen diesen Deklarationen ist, dass XAML eine Sprachdefinition ist. Die Windows-Runtime ist eine Implementierung, die XAML als eine Sprache nutzt und ein spezielles Vokabular definiert, in dem ihre Typen in XAML referenziert werden.

Die XAML-Sprache gibt bestimmte Sprachelemente an, und jedes dieser Elemente sollte über XAML-Verarbeitungsimplementierungen verfügbar sein und für den XAML-Namespace arbeiten. Auf die „x:“-Zuordnungskonvention für den XAML-Sprach-XML-Namespace folgen Projektvorlagen, Beispielcode und die Dokumentation für Sprachfunktionen. Der XAML-Sprachnamespace definiert verschiedene häufig verwendete Funktionen, die selbst für einfache Windows-Runtime-Apps mit C++, C# oder Visual Basic erforderlich sind. Soll z. B. CodeBehind durch eine partielle Klasse einer XAML-Datei hinzugefügt werden, muss die jeweilige Klasse im Stammelement der relevanten XAML-Datei als [x:Class-Attribut](x-class-attribute.md) benannt werden. Oder: In einem beliebigen per XAML-Seite als Schlüsselressource definierten Element in einem [ResourceDictionary- und XAML-Ressourcenverweis](https://msdn.microsoft.com/library/windows/apps/mt187273) muss das [x:Key-Attribut](x-key-attribute.md) auf das relevante Objektelement festgelegt sein.

## Andere XAML-Namespaces

Zusätzlich zum Standardnamespace und dem XAML-Namespace „x:“ der Programmiersprache XAML sind im anfänglichen standardmäßigen XAML-Code für Apps u. U. weitere zugeordnete XAML-Namespaces enthalten, die von Microsoft Visual Studio generiert wurden.

### **d: (`http://schemas.microsoft.com/expression/blend/2008`)**

Der XAML-Namespace „d:“ soll der Designerunterstützung dienen, speziell der Designerunterstützung in den XAML-Entwurfsoberflächen von Microsoft Visual Studio. Der XAML-Namespace „d:“ ermöglicht Designer- und Designzeitattribute in XAML-Elementen. Diese Designerattribute wirken sich lediglich auf die Designaspekte davon aus, wie sich XAML verhält. Die Designerattribute werden ignoriert, wenn derselbe XAML-Code vom Windows-Runtime-XAML-Parser beim Ausführen einer App geladen wird. Im Allgemeinen sind Designerattribute für alle XAML-Elemente gültig, in der Praxis bestehen jedoch nur bestimmte Szenarien, in denen die Anwendung eines Designerattributs durch Sie selbst angebracht ist. Viele der Designerattribute sind insbesondere dazu vorgesehen, eine höhere Benutzerfreundlichkeit bei der Interaktion mit Datenkontexten und Datenquellen zu gewährleisten, während Sie XAML und Code entwickeln, für die bzw. den die Datenbindung verwendet wird.

-   **Attribute „d:DesignHeight“ und „d:DesignWidth“:** Diese Attribute werden gelegentlich auf den Stamm der XAML-Datei angewendet, die von Visual Studio oder einer anderen XAML-Designeroberfläche erstellt wird. Beispielsweise werden diese Attribute am [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647)-Stamm des XAML festgelegt, das beim Hinzufügen eines neuen **UserControl** zu Ihrem App-Projekt erstellt wird. Diese Attribute vereinfachen das Entwerfen der Zusammensetzung des XAML-Inhalts, sodass Sie bereits im Voraus die Layoutbeschränkungen berücksichtigen können, die u. U. vorhanden sind, sobald der XAML-Inhalt für eine Steuerelementinstanz oder einen anderen Teil einer größeren UI-Seite verwendet wird.

   **Hinweis**  Wenn Sie XAML aus Microsoft Silverlight migrieren, sind diese Attribute u. U. in Stammelementen enthalten, die eine gesamte UI-Seite darstellen. In diesem Fall wird empfohlen, die Attribute zu entfernen. Andere Features des XAML-Designers wie der Simulator eignen sich wahrscheinlich besser zum Entwerfen von Seitenlayouts für die Verarbeitung von Skalierungen und Ansichtszuständen als ein Seitenlayout mit fester Größe mit **d:DesignHeight** und **d:DesignWidth**.

-   **d:DataContext-Attribut:** Sie können dieses Attribut für einen Seitenstamm oder ein Steuerelement festlegen, um alle expliziten oder geerbten [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)-Elemente zu überschreiben, die das Objekt ansonsten aufweist.
-   **d:DesignSource-Attribut:** Gibt eine Entwurfszeit-Datenquelle für [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833) an, die [**Source**](https://msdn.microsoft.com/library/windows/apps/br209835) überschreibt.
-   **Markuperweiterungen „d:DesignInstance“ und „d:DesignData“:** Diese Markuperweiterungen werden verwendet, um die Entwurfszeit-Datenressourcen für **d:DataContext** oder **d:DesignSource** bereitzustellen. Die Verwendung von Designzeit-Datenressourcen wird hier nicht vollständig erläutert. Weitere Informationen finden Sie unter [Entwurfszeitattribute](http://go.microsoft.com/fwlink/p/?LinkId=272504). Einige Verwendungsbeispiele finden Sie unter [Beispieldaten für die Entwurfsoberfläche und Prototyperstellung](https://msdn.microsoft.com/library/windows/apps/mt517866).

### **mc: (`http://schemas.openxmlformats.org/markup-compatibility/2006`)**

„mc:“ gibt einen Markupkompatibilitätsmodus zum Lesen von XAML an und unterstützt diesen. In der Regel gehört das „d:“-Präfix zum Attribut **mc:Ignorable**. Diese Methode ermöglicht es Laufzeit-XAML-Parsern, die Designattribute in „d:“ zu ignorieren.

### **local:** und **common:**

„local:ׅ“ ist ein Präfix, das häufig innerhalb der XAML-Seiten für ein vorlagenbasiertes Windows Store-App-Projekt zugeordnet wird. Es wird so zugeordnet, dass es auf denselben Namespace verweist, der erstellt wird, um das [x:Class-Attribut](x-class-attribute.md) und den Code für alle XAML-Dateien aufzunehmen, einschließlich app.xaml. Wenn Sie benutzerdefinierte Klassen für die Verwendung in XAML in diesem Namespace definieren, können Sie mit dem Präfix **local:** auf Ihre benutzerdefinierten Typen in XAML verweisen. Ein zugehöriges Präfix, dass aus einem vorlagenbasierten Windows Store-App-Projekt stammt, ist **common:**. Dieses Präfix verweist auf einen geschachtelten „Common“-Namespace, der Hilfsklassen wie z. B. Konverter und Befehle enthält. Die Definitionen finden Sie im Ordner „Common“ in der **Projektmappen-Explorer**-Ansicht.

### **vsm:**

Nicht verwenden. „vsm:“ ist ein Präfix, das manchmal in älteren XAML-Vorlagen auftaucht, die aus anderen Microsoft-Technologien importiert wurden. Der Namespace diente ursprünglich der Behebung eines Toolproblems mit älteren Namespaces. Sie sollten XAML-Namespacedefinitionen für „vsm:“ im gesamten XAML-Code löschen, den Sie für die Windows-Runtime verwenden. Zudem sollten Sie alle Präfixverwendungen für [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014) und zugehörige Objekte so ändern, dass stattdessen der Standard-XAML-Namespace verwendet wird. Weitere Informationen zur XAML-Migration finden Sie unter [Migrieren von Silverlight- oder WPF-XAML-Code in eine Windows-Runtime-App](https://msdn.microsoft.com/library/windows/apps/br229571).

## Zuordnen von benutzerdefinierten Typen zu XAML-Namespaces und -Präfixen

Sie können einen XAML-Namespace so zuordnen, dass Sie XAML zum Zugriff auf Ihre benutzerdefinierten Typen nutzen können. In anderen Worten: Sie ordnen einen Codenamespace zu, so wie er in der Codedarstellung existiert, die den benutzerdefinierten Typ definiert, und weisen diesem einen XAML-Namespace zusammen mit einem Präfix zur Verwendung zu. Benutzerdefinierte Typen für XAML können entweder in einer Microsoft .NET-Sprache (C# oder Microsoft Visual Basic) oder in C++ definiert werden. Die Zuordnung geschieht über die Definition eines **xmlns**-Präfixes. Beispielsweise definiert `xmlns:myTypes` einen neuen XAML-Namespace, auf den zugegriffen wird, indem alle Verwendungen mit einem Präfix versehen werden, nämlich dem Token `myTypes:`.

Eine **xmlns**-Definition enthält einen Wert und den Präfixnamen. Der Wert entspricht einer Zeichenfolge, die in Anführungszeichen gesetzt wird und auf ein Gleichheitszeichen folgt. Eine gängige XML-Konvention ist die Zuordnung einer URI (Uniform Resource Identifier) zum XML-Namespace, sodass eine Konvention für Eindeutigkeit und Identifikation besteht. Diese Konvention besteht auch für den Standard-XML-Namespace und den XAML-Sprach-XAML-Namespace, zudem für einige weniger häufig eingesetzte XAML-Namespaces, die von Windows-Runtime-XAML verwendet werden. Jedoch wird bei einem XAML-Namespace zur Zuordnung benutzerdefinierter Typen keine URI angegeben, sondern die Präfixdefinition mit dem Token „using:“ begonnen. Nach dem Token „using:“ wird der Codenamespace benannt.

Um beispielsweise ein „custom1“-Präfix zuzuordnen, das Ihnen die Referenzierung eines „CustomClasses“-Namespace und die Verwendung der Klassen von diesem Namespace oder dieser Assembly als Objektelemente in XAML ermöglicht, sollte Ihre XAML-Seite die folgenden Zuordnungen im Stammelement aufweisen: `xmlns:custom1="using:CustomClasses"`

Partielle Klassen desselben Seitenbereichs müssen nicht zugeordnet werden. Beispielsweise benötigen Sie keine Präfixe, um beliebige Ereignishandler zu referenzieren, die Sie zur Handhabung von Ereignissen von der XAML-Oberflächendefinition Ihrer Seite definierten haben. Darüber hinaus ordnen viele der Start-XAML-Seiten der von Visual Studio generierten Projekte für Windows-Runtime-Apps mit C++, C# oder Visual Basic bereits ein „local:“-Präfix zu, das den im Projekt angegebenen Standardnamespace und den Namespace referenziert, der von Definitionen partieller Klassen verwendet wird.

### CLR-Sprachregeln

Wenn Sie Ihren Unterstützungscode in einer .NET-Sprache (C# oder Microsoft Visual Basic) schreiben, richten Sie sich möglicherweise nach einer Konvention, wonach ein Punkt („.“) im Namespacenamen enthalten ist, sodass eine konzeptionelle Hierarchie von Codenamespaces entsteht. Wenn Ihre Namespacedefinition einen Punkt enthält, sollte der Punkt Teil des Werts sein, den Sie nach dem Token „using:“ angeben.

Wenn Ihre CodeBehind-Datei oder Codedefinitionsdatei eine C++-Datei ist, richten sich bestimmte Konventionen trotzdem nach der CLR-Sprachform (Common Language Runtime), damit keine Abweichungen in der XAML-Syntax auftreten. Wenn Sie geschachtelte Namespaces in C++ deklarieren, sollte das Trennzeichen zwischen den aufeinander folgenden geschachtelten Zeichenfolgen „.“ statt „::“ sein, wenn Sie den Wert angeben, der auf das Token „using:“ folgt.

Verwenden Sie keine geschachtelten Typen (z. B. Schachtelung einer Enumeration innerhalb einer Klasse), wenn Sie Ihren Code für die Verwendung mit XAML definieren. Geschachtelte Typen können nicht ausgewertet werden. Für den XAML-Parser besteht keine Möglichkeit, zu erkennen, ob ein Punkt Teil des Namens des geschachtelten Typs oder Teil des Namespace-Namens ist.

## Benutzerdefinierte Typen und Assemblys

Der Name der Assembly, die die Unterstützungstypen für einen XAML-Namespace definiert, wird in der Zuordnung nicht angegeben. Die Logik, für die Assemblys zur Verfügung stehen, wird auf App-Definitionsebene gesteuert und ist Teil von grundlegenden App-Bereitstellungs- und -Sicherheitsprinzipien. Deklarieren Sie alle Assemblys, die als Codedefinitionsquelle für XAML eingeschlossen werden sollen, als abhängige Assemblys in den Projekteinstellungen. Weitere Informationen finden Sie unter [Erstellen von Windows-Runtime-Komponenten in C# und Visual Basic](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx).

Wenn Sie benutzerdefinierte Typen von der Anwendungsdefinition oder Seitendefinition der Haupt-App referenzieren, stehen diese Typen ohne eine weitere Konfiguration abhängiger Assemblys zur Verfügung, der Codenamespace mit diesen Typen muss jedoch auch weiterhin zugeordnet werden. Eine häufige Konvention ist, das Präfix „local“ für den Standardcodenamespace jeder relevanten XAML-Seite zuzuordnen. Diese Konvention wird oft in Startprojektvorlagen für XAML-Projekte aufgenommen.

## Angefügte Eigenschaften

Wenn Sie auf angefügte Eigenschaften verweisen, muss der Besitzer-Typ-Teil der angefügten Eigenschaft entweder im standardmäßigen XAML-Namespace enthalten sein oder ein Präfix aufweisen. Das getrennte Hinzufügen von Präfixen zu Attributen und ihren Elementen kommt selten vor, in diesem Fall ist es jedoch manchmal erforderlich, insbesondere für eine benutzerdefinierte angefügte Eigenschaft. Weitere Informationen finden Sie unter [Benutzerdefinierte angefügte Eigenschaften](custom-attached-properties.md).

## Verwandte Themen

* [Übersicht über XAML](xaml-overview.md)
* [Anleitung zur XAML-Syntax](xaml-syntax-guide.md)
* [Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [C#-, VB- und C++-Projektvorlagen für Windows-Runtime-Apps](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [Migrieren von Silverlight- oder WPF XAML-Code in Windows-Runtime-Apps](https://msdn.microsoft.com/library/windows/apps/br229571)
 



<!--HONumber=May16_HO2-->


