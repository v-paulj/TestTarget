---
Description: Beschreibt das Konzept von Automatisierungspeers für die Microsoft-Benutzeroberflächenautomatisierung und erläutert, wie Sie eine Unterstützung der Benutzeroberflächenautomatisierung für eine eigene benutzerdefinierte UI-Klasse bereitstellen können.
title: Benutzerdefinierte Automatisierungspeers
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
label: Custom automation peers
template: detail.hbs
---

Benutzerdefinierte Automatisierungspeers
===================================================================================

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Beschreibt das Konzept von Automatisierungspeers für die Microsoft-Benutzeroberflächenautomatisierung und erläutert, wie Sie eine Unterstützung der Benutzeroberflächenautomatisierung für eine eigene benutzerdefinierte UI-Klasse bereitstellen können.

Mit der Benutzeroberflächenautomatisierung wird ein Framework bereitgestellt, das Automatisierungsclients verwenden können, um die Benutzeroberflächen einer Vielzahl von UI-Plattformen und Frameworks zu untersuchen oder zu betreiben. Wenn Sie eine App für die universelle Windows-Plattform schreiben, bieten die Klassen, die Sie für Ihre Benutzeroberfläche verwenden, bereits Unterstützung für Benutzeroberflächenautomatisierung. Sie können vorhandene nicht versiegelte -Klassen ableiten, um einen neuen UI-Steuerelementtyp oder eine Unterstützungsklasse zu definieren. Hierbei kann mit Ihrer Klasse möglicherweise Verhalten zur Unterstützung der Barrierefreiheit hinzugefügt werden, das die Standard-Benutzeroberflächenautomatisierung nicht bereitstellt. In diesem Fall sollten Sie die vorhandene Unterstützung der Benutzeroberflächenautomatisierung erweitern, indem Sie von der [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)-Klasse ableiten, die in der Basisimplementierung verwendet wird. Zudem sollten Sie Ihrer Peerimplementierung die notwendige Unterstützung hinzufügen und die Steuerungsinfrastruktur der Universelle Windows-Plattform (UWP)-App anweisen, den neuen Peer zu erstellen.

Die Benutzeroberflächenautomatisierung ermöglicht nicht nur Anwendungen für die Barrierefreiheit und Hilfstechnologien wie Bildschirmleseprogramme, sondern auch Qualitätssicherungscode (Testcode). In beiden Szenarien können Benutzeroberflächenautomatisierungs-Clients die Benutzeroberflächenelemente untersuchen und die Benutzerinteraktion mit Ihrer App für Code simulieren, der nicht aus Ihrer App stammt. Informationen zur plattformübergreifenden Benutzeroberflächenautomatisierung und ihrer weiteren Bedeutung finden Sie unter [Übersicht über die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee684076).

Es gibt zwei unterschiedliche Zielgruppen, von denen das Benutzeroberflächenautomatisierungs-Framework verwendet wird.

-   Von *Clients* für die Benutzeroberflächenautomatisierung werden Benutzeroberflächenautomatisierungs-APIs aufgerufen, um Informationen zur gesamten gegenwärtig für den Benutzer angezeigten Benutzeroberfläche abzurufen. Beispielsweise fungiert eine Hilfstechnologie wie etwa eine Bildschirmsprachausgabe als Benutzeroberflächenautomatisierungs-Client. Die Benutzeroberfläche wird als Struktur aus verwandten Automatisierungselementen dargestellt. Der Benutzeroberflächenautomatisierungs-Client benötigt u. U. Informationen zu jeweils nur einer App oder aber zur gesamten Struktur. Vom Benutzeroberflächenautomatisierungs-Client können Benutzeroberflächenautomatisierungs-APIs zum Navigieren in der Struktur und zum Lesen oder Ändern von Informationen in den Automatisierungselementen verwendet werden.
-   Von *Anbietern* für die Benutzeroberflächenautomatisierung werden Informationen zum Benutzeroberflächenautomatisierungs-Baum hinzugefügt. Dies erfolgt durch die Implementierung von APIs, von denen die Elemente in der Benutzeroberfläche verfügbar gemacht werden, die als Teil der App eingeführt wurde. Wenn Sie ein neues Steuerelement erstellen, sollten Sie jetzt als Teilnehmer des Benutzeroberflächenautomatisierungs-Anbieterszenarios fungieren. Als Anbieter sollten Sie aus Gründen der Barrierefreiheit und zu Testzwecken sicherstellen, dass alle Benutzeroberflächenautomatisierungs-Clients das Benutzeroberflächenautomatisierungs-Framework für die Interaktion mit Ihrem Steuerelement nutzen können.

Gewöhnlich sind im Benutzeroberflächenautomatisierungs-Framework parallele APIs vorhanden: eine API für Benutzeroberflächenautomatisierungs-Clients und eine andere API mit einem ähnlichen Namen für Benutzeroberflächenautomatisierungs-Anbieter. In diesem Thema geht es hauptsächlich um die APIs für den Benutzeroberflächenautomatisierungs-Anbieter und speziell um die Klassen und Schnittstellen, die eine Anbietererweiterbarkeit in diesem Benutzeroberflächenframework ermöglichen. Gelegentlich werden auch von Benutzeroberflächenautomatisierungs-Clients verwendete Benutzeroberflächenautomatisierungs-APIs angesprochen, um eine andere Perspektive zu bieten oder in einer Suchtabelle einen Vergleich von Client- und Anbieter-APIs zu veranschaulichen. Weitere Informationen zur Clientperspektive finden Sie im [Programmierhandbuch für Benutzeroberflächenautomatisierungs-Clients](https://msdn.microsoft.com/library/windows/desktop/Ee684021).

**Hinweis**  
Benutzeroberflächenautomatisierungs-Clients verwenden normalerweise keinen verwalteten Code und werden meist auch nicht als UWP-App implementiert (sondern als Desktop-Apps). Die Benutzeroberflächenautomatisierung basiert auf einem Standard, nicht auf einer bestimmten Implementierung oder einem bestimmten Framework. Viele vorhandene Benutzeroberflächenautomatisierungs-Clients, z. B. Hilfstechnologieprodukte wie Bildschirmleseprogramme, verwenden Component Object Model (COM)-Schnittstellen für die Interaktion mit der Benutzeroberflächenautomatisierung, dem System und den in untergeordneten Fenstern ausgeführten Apps. Weitere Informationen zu COM-Schnittstellen und zum Schreiben eines Benutzeroberflächenautomatisierungs-Clients mithilfe von COM finden Sie unter [Grundlagen der Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee684007).

 

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"></span><span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"></span><span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"></span>Ermitteln des vorhandenen Zustands der Benutzeroberflächenautomatisierung für Ihre benutzerdefinierte UI-Klasse
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Bevor Sie versuchen, einen Automatisierungspeer für ein benutzerdefiniertes Steuerelement zu implementieren, sollten Sie testen, ob die Basisklasse und der Automatisierungspeer bereits die erforderliche Barrierefreiheit oder Automatisierungsunterstützung bereitstellen. In vielen Fällen kann die Kombination aus [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472)-Implementierungen, bestimmten Peers und den Mustern, die diese implementieren, eine einfache Barrierefreiheit bieten, die den Anforderungen genügt. Dies hängt davon ab, wie viele Änderungen Sie an der Objektmodellverfügbarkeit Ihres Steuerelements im Vergleich zur Basisklasse vorgenommen haben. Zudem hängt dies davon ab, ob Ihre Ergänzungen der Basisklassenfunktionalität mit den neuen UI-Elementen im Vorlagenvertrag oder mit der visuellen Darstellung des Steuerelements korrelieren. In einigen Fällen führen Ihre Änderungen möglicherweise zu neuen Aspekten der Benutzererfahrung, die eine zusätzliche Unterstützung der Barrierefreiheit erfordern.

Auch wenn durch die Verwendung der vorhandenen Basispeerklasse eine grundlegende Unterstützung für die Barrierefreiheit bereitgestellt wird, empfiehlt es sich, einen Peer zu definieren. So können Sie der Benutzeroberflächenautomatisierung genaue **ClassName**-Informationen für automatisierte Testszenarien angeben. Diese Überlegung ist besonders wichtig, wenn Sie ein Steuerelement für den Einsatz in Drittanbieterlösungen entwerfen.

<span id="Automation_peer_classes__"></span><span id="automation_peer_classes__"></span><span id="AUTOMATION_PEER_CLASSES__"></span>Automatisierungspeerklassen
-----------------------------------------------------------------------------------------------------------------------------------------------------------

Die UWP baut auf vorhandenen Techniken und Konventionen für die Benutzeroberflächenautomatisierung auf, die von vorherigen Benutzeroberflächenframeworks mit verwaltetem Code verwendet wurden, z. B. Windows Forms, Windows Presentation Foundation (WPF) und Microsoft Silverlight. Viele der Steuerelementklassen und deren Funktion und Zweck entstammen auch einem vorherigen Benutzeroberflächenframework.

Üblicherweise beginnen Peerklassennamen mit dem Namen der Steuerelementklasse und enden mit „AutomationPeer“. Beispielsweise ist [**ButtonAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242458) die Peerklasse für die [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Steuerelementklasse.

**Hinweis**  Im Rahmen dieses Themas räumen wir den Eigenschaften, die sich auf die Barrierefreiheit beziehen, beim Implementieren eines Peers für ein Steuerelement eine höhere Priorität ein. In einem allgemeineren Konzept für die Unterstützung der Benutzeroberflächenautomatisierung sollten Sie jedoch einen Peer gemäß den Empfehlungen im [Programmierhandbuch für Benutzeroberflächenautomatisierungs-Anbieter](https://msdn.microsoft.com/library/windows/desktop/Ee671596) und in den [Grundlagen der Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee684007) implementieren. In diesen Themen werden zwar keine spezifischen [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)-APIs behandelt, die Sie zum Bereitstellen der Informationen im UWP-Framework für die Benutzeroberflächenautomatisierung verwenden können, aber es werden die Eigenschaften beschrieben, mit denen Ihre Klasse identifiziert wird oder andere Informationen oder Interaktionen bereitgestellt werden.

 

<span id="Peers__patterns_and_control_types"></span><span id="peers__patterns_and_control_types"></span><span id="PEERS__PATTERNS_AND_CONTROL_TYPES"></span>Peers, Muster und Steuerelementtypen
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Ein *Steuerelementmuster* ist eine Schnittstellenimplementierung, die einen bestimmten Aspekt der Funktionalität eines Steuerelements für einen Benutzeroberflächenautomatisierungs-Client verfügbar macht. Benutzeroberflächenautomatisierungs-Clients verwenden die Eigenschaften und Methoden, die über ein Steuerelementmuster verfügbar gemacht werden, um Informationen über die Funktionen des Steuerelements abzurufen oder das Verhalten des Steuerelements zur Laufzeit zu ändern.

Steuerelementmuster ermöglichen es, die Funktionen eines Steuerelements unabhängig vom Typ oder vom Aussehen des Steuerelements zu kategorisieren und verfügbar zu machen. Ein Steuerelement, das eine tabellarische Oberfläche darstellt, verwendet das **Grid**-Steuerelementmuster, um die Anzahl der Zeilen und Spalten in der Tabelle verfügbar zu machen und einem Benutzeroberflächenautomatisierungs-Client zu ermöglichen, Objekte aus der Tabelle abzurufen. Der Benutzeroberflächenautomatisierungs-Client kann das **Invoke**-Steuerelementmuster für aufrufbare Steuerelemente (z. B. Schaltflächen) und das **Scroll**-Steuerelementmuster für Steuerelemente mit Bildlaufleisten (z. B. Listenfelder, Listenansichten oder Kombinationsfelder) verwenden. Jedes Steuerelementmuster stellt eine separate Funktion dar, und Steuerelementmuster können kombiniert werden, um den gesamten Funktionsumfang zu beschreiben, der von einem bestimmten Steuerelement unterstützt wird.

Steuerelementmuster verhalten sich zur Benutzeroberfläche wie Schnittstellen zu COM-Objekten. In COM können Sie von einem Objekt abfragen, welche Schnittstellen es unterstützt, und dann mithilfe dieser Schnittstellen auf die Funktionen zugreifen. Bei der Benutzeroberflächenautomatisierung können Benutzeroberflächenautomatisierungs-Clients von einem Benutzeroberflächenautomatisierungs-Element abfragen, welche Steuerelementmuster es unterstützt, und dann über die vom unterstützten Steuerelementmuster verfügbar gemachten Eigenschaften, Methoden, Ereignisse und Strukturen mit dem Element und dem Peersteuerelement interagieren.

Eine der Hauptaufgaben eines Automatisierungspeers besteht darin, dem Benutzeroberflächenautomatisierungs-Client zu melden, welche Steuerelementmuster das UI-Element über seinen Peer unterstützen kann. Hierzu werden von Benutzeroberflächenautomatisierungs-Anbietern neue Peers implementiert, mit denen das Verhalten der [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpattern)-Methode durch Überschreiben der [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpatterncore)-Methode geändert wird. Benutzeroberflächenautomatisierungs-Clients führen Aufrufe durch, die vom Benutzeroberflächenautomatisierungs-Anbieter dem aufrufenden **GetPattern**-Element zugeordnet werden. Benutzeroberflächenautomatisierungs-Clients fragen jedes spezielle Muster ab, mit dem sie interagieren möchten. Wenn der Peer das Muster unterstützt, gibt er einen Objektverweis auf sich selbst oder andernfalls **null** zurück. Wird nicht **null** zurückgegeben, erwartet der Benutzeroberflächenautomatisierungs-Client, dass er APIs der Musterschnittstelle als Client aufrufen kann, um mit diesem Steuerelementmuster zu interagieren.

Ein *Steuerelementtyp* ist eine Möglichkeit, die Funktionalität eines vom Peer dargestellten Steuerelements allgemein zu definieren. Dieses Konzept unterscheidet sich insofern von dem eines Steuerelementmusters, da ein Muster die Benutzeroberflächenautomatisierung informiert, welche Informationen sie abrufen kann oder welche Aktionen sie über eine bestimmte Schnittstelle ausführen kann, während sich der Steuerelementtyp eine Ebene höher befindet. Jeder Steuerelementtyp verfügt über Informationen zu den folgenden Aspekten der Benutzeroberflächenautomatisierung:

-   Steuerelementmuster für die Benutzeroberflächenautomatisierung: Ein Steuerelementtyp kann mehrere Muster unterstützen, die jeweils eine andere Klassifizierung von Informationen oder Interaktionen darstellen. Jeder Steuerelementtyp verfügt über einen Satz von Steuerelementmustern, die das Steuerelement unterstützen muss. Dabei ist ein Satz optional, und der andere darf vom Steuerelement nicht unterstützt werden.
-   Eigenschaftswerte für die Benutzeroberflächenautomatisierung: Jeder Steuerelementtyp hat einen Satz von Eigenschaften, die das Steuerelement unterstützen muss. Dabei handelt es sich um die allgemeinen Eigenschaften, die unter [Übersicht über Eigenschaften für die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee671594) beschrieben werden, nicht um die musterspezifischen Eigenschaften.
-   Ereignisse für die Benutzeroberflächenautomatisierung: Jeder Steuerelementtyp hat einen Satz von Ereignissen, die das Steuerelement unterstützen muss. Auch hier handelt es sich nicht um musterspezifische Ereignisse, sondern um die unter [Übersicht über Ereignisse für die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee671221) beschriebenen Ereignisse.
-   Baumstruktur der Benutzeroberflächenautomatisierung: Jeder Steuerelementtyp definiert, wie das Steuerelement in der Baumstruktur der Benutzeroberflächenautomatisierung dargestellt werden muss.

Unabhängig von der Implementierungsweise von Automatisierungspeers für das Framework sind die Funktionen von Benutzeroberflächenautomatisierungs-Clients nicht an die UWP gebunden. Es ist sogar wahrscheinlich, dass vorhandene Benutzeroberflächenautomatisierungs-Clients wie beispielsweise Hilfstechnologien andere Programmiermodelle (z. B. COM) verwenden. In COM können Clients **QueryInterface** für die Schnittstelle des COM-Steuerelementmusters, das das angeforderte Muster implementiert, oder das allgemeine Benutzeroberflächenautomatisierungs-Framework für Eigenschaften, Ereignisse oder die Strukturuntersuchung verwenden. Mithilfe des Benutzeroberflächenautomatisierungs-Frameworks wird für die Muster das Marshalling in UWP-Code vorgenommen, der für den Benutzeroberflächenautomatisierungs-Anbieter der App und den entsprechenden Peer ausgeführt wird.

Wenn Sie Steuerelementmuster für ein Framework mit verwaltetem Code implementieren, z. B. eine Windows Store-App mit C\# oder Microsoft Visual Basic, können Sie für die Darstellung dieser Muster .NET Framework-Schnittstellen anstelle von COM-Schnittstellen verwenden. Beispielsweise ist die Benutzeroberflächenautomatisierungs-Musterschnittstelle für eine Microsoft .NET-Anbieterimplementierung des **Invoke**-Musters [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582).

Eine Liste mit Steuerelementmustern, Anbieterschnittstellen und deren Zweck finden Sie unter [Steuerelementmuster und -schnittstellen](control-patterns-and-interfaces.md). Eine Liste der Steuerelementtypen finden Sie unter [Übersicht über Steuerelementtypen für die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee671197).

### <span id="Guidance_for_how_to_implement_control_patterns"></span><span id="guidance_for_how_to_implement_control_patterns"></span><span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"></span>Leitfaden für die Implementierung von Steuerelementmustern

Die Steuerelementmuster und ihr jeweiliger Zweck sind Bestandteil einer umfangreicheren Definition des Benutzeroberflächenautomatisierungs-Frameworks und gelten nicht nur für die Unterstützung der Barrierefreiheit in einer Windows Store-App. Stellen Sie beim Implementieren eines Steuerelementmusters sicher, dass die Implementierung den auf MSDN und in der Spezifikation für die Benutzeroberflächenautomatisierung dokumentierten Richtlinien entspricht. Wenn Sie Unterstützung benötigen, können Sie im Allgemeinen die MSDN-Themen verwenden und müssen nicht die Spezifikation zurate ziehen. Einen Leitfaden für die einzelnen Muster finden Sie unter [Implementieren von Steuerelementmustern für die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee671292). Jedes Thema in diesem Bereich enthält einen Abschnitt mit Implementierungsrichtlinien und -konventionen und einen mit den erforderlichen Membern. Die Richtlinien beziehen sich in der Regel auf bestimmte APIs der relevanten Steuerelementmuster-Schnittstelle in der Referenz [Steuerelementmuster-Schnittstellen für Anbieter](https://msdn.microsoft.com/library/windows/desktop/Ee671201). Bei diesen Schnittstellen handelt es sich um die systemeigenen Schnittstellen bzw. COM-Schnittstellen (und die zugehörigen APIs verwenden eine Syntax im COM-Stil). Für alles, was Sie hier sehen, gibt es jedoch eine Entsprechung im [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225)-Namespace.

Wenn Sie die Standardautomatisierungspeers verwenden und deren Verhalten erweitern, ist zu beachten, dass diese Peers bereits gemäß den Richtlinien für die Benutzeroberflächenautomatisierung für Anbieter geschrieben wurden. Wenn sie Steuerelementmuster unterstützen, können Sie sich darauf verlassen, dass die jeweiligen Muster den Richtlinien unter [Implementieren von Steuerelementmustern für die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee671292) entsprechen. Wenn ein Steuerelementpeer meldet, dass er einen durch die Benutzeroberflächenautomatisierung definierten Steuerelementtyp darstellt, orientiert sich dieser Peer an den unter [Unterstützen von Steuerelementtypen für die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee671633) dokumentierten Richtlinien.

Trotzdem benötigen Sie u. U. weitere Richtlinien für Steuerelementmuster oder -typen, um in Ihrer Peerimplementierung den Empfehlungen für die Benutzeroberflächenautomatisierung zu folgen. Dies ist besonders dann der Fall, wenn Sie eine Unterstützung für Muster oder Steuerelementtypen implementieren, die noch nicht als Standardimplementierung in einem UWP-Steuerelement vorhanden ist. Das Muster für Anmerkungen ist z. B. in keinem der standardmäßigen XAML-Steuerelemente implementiert. Vielleicht haben Sie aber eine App, die Anmerkungen intensiv nutzt, und möchten daher den Zugriff auf diese Funktionalität ermöglichen. Für dieses Szenario sollte Ihr Peer [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493) implementieren und sich vermutlich als **Document**-Steuerelementtyp mit entsprechenden Eigenschaften melden. Dies ist ein Hinweis darauf, dass Ihre Dokumente Anmerkungen unterstützen.

Wir empfehlen, die Ratschläge für die Muster unter [Implementieren von Steuerelementmustern für die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee671292) oder für Steuerelementtypen unter [Unterstützen von Steuerelementtypen für die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee671633) zur Orientierung und als allgemeinen Leitfaden zu nutzen. Sie können auch einigen der API-Links folgen, unter denen Sie Beschreibungen und Anmerkungen zum Zweck der APIs finden. Einzelheiten zur speziellen Syntax für die Programmierung von UWP-Apps finden Sie unter der entsprechenden API im [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225)-Namespace und den zugehörigen Referenzseiten.

<span id="Built-in_automation_peer_classes"></span><span id="built-in_automation_peer_classes"></span><span id="BUILT-IN_AUTOMATION_PEER_CLASSES"></span>Integrierte Automatisierungspeerklassen
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Im Allgemeinen implementieren Elemente eine Automatisierungspeerklasse, wenn sie UI-Aktivität vom Benutzer akzeptieren. Ein weiterer Fall ist das Vorhandensein von Informationen, die Benutzer von Hilfstechnologien benötigen, mit denen die interaktive oder aussagekräftige Benutzeroberfläche von Apps dargestellt wird. Nicht alle visuellen UWP-Elemente verfügen über Automatisierungspeers. Beispiele für Klassen, die Automatisierungspeers implementieren, sind [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) und [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683). Beispiele für Klassen, die keine Automatisierungspeers implementieren, sind [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) und Klassen auf Basis von [**Panel**](https://msdn.microsoft.com/library/windows/apps/BR227511), z. B. [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) und [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267). Ein **Panel** weist keinen Peer auf, weil es nur ein visuelles Layoutverhalten bereitstellt. Benutzer haben keine für die Barrierefreiheit relevante Möglichkeit, mit einem **Panel** zu interagieren. Unabhängig davon, welche untergeordneten Elemente ein **Panel** enthält, werden diese den Benutzeroberflächenautomatisierungs-Bäumen als untergeordnete Elemente des nächsten verfügbaren übergeordneten Elements in der Struktur gemeldet, das über eine Peer- oder Elementdarstellung verfügt.

<span id="UI_Automation_and_UWP_process_boundaries"></span><span id="ui_automation_and_uwp_process_boundaries"></span><span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"></span>Benutzeroberflächenautomatisierung und UWP-Prozessgrenzen
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

In der Regel wird Benutzeroberflächenautomatisierungs-Client-Code, der auf eine UWP-App zugreift, außerhalb des Prozesses ausgeführt. Die Infrastruktur des Benutzeroberflächenautomatisierungs-Frameworks ermöglicht es, Informationen über Prozessgrenzen hinweg zu übertragen. Weitere Informationen zu diesem Konzept finden Sie in den [Grundlagen der Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee684007).

<span id="OnCreateAutomationPeer"></span><span id="oncreateautomationpeer"></span><span id="ONCREATEAUTOMATIONPEER"></span>OnCreateAutomationPeer
-------------------------------------------------------------------------------------------------------------------------------------------------

Alle Klassen, die von [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) abgeleitet werden, enthalten die geschützte virtuelle Methode [**OnCreateAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR208911_oncreateautomationpeer). Die Objektinitialisierungssequenz für Automatisierungspeers ruft **OnCreateAutomationPeer** auf, um das Automatisierungspeerobjekt für jedes Steuerelement abzurufen und so einen Benutzeroberflächenautomatisierungs-Baum zur Laufzeitverwendung zu erstellen. Der Benutzeroberflächenautomatisierungs-Code kann den Peer verwenden, um Informationen über die Merkmale und Features eines Steuerelements abzurufen und mithilfe der Steuerelementmuster die interaktive Verwendung des Steuerelements zu simulieren. Ein benutzerdefiniertes Steuerelement, das Automatisierung unterstützt, muss **OnCreateAutomationPeer** überschreiben und eine Instanz der Klasse zurückgeben, die von [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) abgeleitet wird. Wenn beispielsweise ein benutzerdefiniertes Steuerelement von der Klasse [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/BR227736) abgeleitet wird, sollte das von **OnCreateAutomationPeer** zurückgegebene Objekt von [**ButtonBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242460) abgeleitet werden.

Wenn Sie eine benutzerdefinierte Steuerelementklasse schreiben und außerdem einen neuen Automatisierungspeer bereitstellen möchten, sollten Sie die [**OnCreateAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR208911_oncreateautomationpeer)-Methode für das benutzerdefinierte Steuerelement überschreiben, sodass eine neue Instanz Ihres Peers zurückgegeben wird. Die Peerklasse muss direkt oder indirekt von [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) abgeleitet werden.

Mit dem folgenden Code wird beispielsweise deklariert, dass das benutzerdefinierte `NumericUpDown`-Steuerelement den Peer `NumericUpDownPeer` zur Benutzeroberflächenautomatisierung verwenden soll.

<span codelanguage="CSharp"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>using Windows.UI.Xaml.Automation.Peers;
...
public class NumericUpDown : RangeBase {
    public NumericUpDown() {
    // other initialization; DefaultStyleKey etc.
    }
    ...
    protected override AutomationPeer OnCreateAutomationPeer()
    {
        return new NumericUpDownAutomationPeer(this);
    }
}</code></pre></td>
</tr>
</tbody>
</table>

<span codelanguage="VisualBasic"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">VB</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>Public Class NumericUpDown
    Inherits RangeBase
    &#39; other initialization; DefaultStyleKey etc.
       Public Sub New()
       End Sub
       Protected Overrides Function OnCreateAutomationPeer() As AutomationPeer
              Return New NumericUpDownAutomationPeer(Me)
       End Function
End Class</code></pre></td>
</tr>
</tbody>
</table>

<span codelanguage="ManagedCPlusPlus"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase 
{
// other initialization not shown
protected: 
    virtual AutomationPeer^ OnCreateAutomationPeer() override 
    { 
         return ref new NumericUpDown(this); 
    } 
};</code></pre></td>
</tr>
</tbody>
</table>

**Hinweis**  Es wird empfohlen, dass die [**OnCreateAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR208911_oncreateautomationpeer)-Implementierung lediglich eine neue Instanz des benutzerdefinierten Automatisierungspeers initialisiert, das aufrufende Steuerelement als Besitzer übergibt und diese Instanz zurückgibt. Versuchen Sie nicht, in diese Methode zusätzliche Logik einzubinden. Besonders Logik, die möglicherweise zur Zerstörung des [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)-Elements innerhalb desselben Aufrufs führt, kann ein unvorhergesehenes Laufzeitverhalten zur Folge haben.

 

In typischen Implementierungen von [**OnCreateAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR208911_oncreateautomationpeer) ist *owner* als **this** oder **Me** angegeben, da die Überschreibung der Methode den gleichen Umfang wie der Rest der Steuerelement-Klassendefinition hat.

Die tatsächliche Peerklassendefinition kann in derselben Codedatei wie das Steuerelement oder in einer separaten Codedatei erfolgen. Die Peerdefinitionen befinden sich alle im [**Windows.UI.Xaml.Automation.Peers**](https://msdn.microsoft.com/library/windows/apps/BR242563)-Namespace, also getrennt von den Steuerelementen, für die sie Peers bereitstellen. Sie können Ihre Peers auch in separaten Namespaces deklarieren, sofern Sie auf die erforderlichen Namespaces für den [**OnCreateAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR208911_oncreateautomationpeer)-Methodenaufruf verweisen.

### <span id="Choosing_the_correct_peer_base_class"></span><span id="choosing_the_correct_peer_base_class"></span><span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"></span>Auswählen der richtigen Peerbasisklasse

Stellen Sie sicher, dass Ihr [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) von der Basisklasse abgeleitet wird, die am besten mit der vorhandenen Peerlogik der Steuerelementklasse übereinstimmt, von der Sie ableiten. Da `NumericUpDown` von [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863) abgeleitet wird, ist im vorherigen Beispiel eine [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506)-Klasse verfügbar, auf der Ihr Peer basieren sollte. Indem Sie entsprechend der Art, wie Sie das Steuerelement selbst ableiten, die am besten passende Peerklasse verwenden, können Sie zumindest für einen Teil der [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590)-Funktionalität eine Überschreibung vermeiden, da diese bereits von der Basispeerklasse implementiert wird.

Die [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390)-Basisklasse besitzt keine entsprechende Peerklasse. Wenn Sie eine Peerklasse benötigen, die mit einem benutzerdefinierten Steuerelement übereinstimmt, das von **Control** abgeleitet wird, leiten Sie die benutzerdefinierte Peerklasse von [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) ab.

Wenn Sie direkt von [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) ableiten, hat diese Klasse kein standardmäßiges Automatisierungspeerverhalten, da keine [**OnCreateAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR208911_oncreateautomationpeer)-Implementierung vorhanden ist, die auf eine Peerklasse verweist. Implementieren Sie also entweder **OnCreateAutomationPeer**, um Ihren eigenen Peer zu verwenden, oder verwenden Sie [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) als Peer, wenn die hiermit bereitgestellte Unterstützung für die Barrierefreiheit für Ihr Steuerelement ausreichend ist.

**Hinweis**  In der Regel führen Sie keine Ableitungen von [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) anstelle von [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) durch. Beim direkten Ableiten von **AutomationPeer** müssen Sie einen Großteil der grundlegenden Unterstützung der Barrierefreiheit duplizieren, die sonst über **FrameworkElementAutomationPeer** bereitgestellt wird.

 

<span id="Initialization_of_a_custom_peer_class"></span><span id="initialization_of_a_custom_peer_class"></span><span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"></span>Initialisieren einer benutzerdefinierten Peerklasse
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Der Automatisierungspeer sollte einen typsicheren Konstruktor definieren, der eine Instanz des Besitzersteuerelements für die Basisinitialisierung verwendet. Im nächsten Beispiel übergibt die Implementierung den *owner*-Wert an die [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506)-Basis (und letztendlich verwendet tatsächlich [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) *owner*, um [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472_owner) festzulegen).

<span codelanguage="CSharp"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}</code></pre></td>
</tr>
</tbody>
</table>

<span codelanguage="VisualBasic"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">VB</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub</code></pre></td>
</tr>
</tbody>
</table>

<span codelanguage="ManagedCPlusPlus"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer 
//.cpp
public:  
    NumericUpDownAutomationPeer(NumericUpDown^ owner);</code></pre></td>
</tr>
</tbody>
</table>

<span id="Core_methods_of_AutomationPeer"></span><span id="core_methods_of_automationpeer"></span><span id="CORE_METHODS_OF_AUTOMATIONPEER"></span>Hauptmethoden von AutomationPeer
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Aufgrund der UWP-Infrastruktur sind die überschreibbaren Methoden eines Automatisierungspeers Teil eines Methodenpaars: der öffentlichen Zugriffsmethode, die der Benutzeroberflächenautomatisierungs-Anbieter als Weiterleitungspunkt für Benutzeroberflächenautomatisierungs-Clients verwendet, und der geschützten „Core“-Anpassungsmethode, die von einer UWP-Klasse überschrieben werden kann, um das Verhalten zu beeinflussen. Das Methodenpaar wird standardmäßig so gebündelt, dass mit dem Aufruf der Zugriffsmethode stets die parallele „Core“-Methode mit der Anbieterimplementierung oder (als Fallback) eine Standardimplementierung aus den Basisklassen aufgerufen wird.

Überschreiben Sie beim Implementieren eines Peers für ein benutzerdefiniertes Steuerelement eine beliebige „Core“-Methode aus der Basis-Automatisierungspeerklasse, in der Sie für Ihr benutzerdefiniertes Steuerelement spezifisches Verhalten verfügbar machen möchten. Der Benutzeroberflächenautomatisierungs-Code ruft durch Aufruf der öffentlichen Methoden der Peerklasse Informationen über Ihr Steuerelement ab. Um Informationen zu Ihrem Steuerelement bereitzustellen, überschreiben Sie jede Methode, deren Name auf „Core“ endet, wenn durch die Implementierung und das Design Ihres Steuerelements Barrierefreiheitsszenarien oder andere Benutzeroberflächenautomatisierungs-Szenarien erstellt werden, die von jenen abweichen, die von der Basis-Automatisierungspeerklasse unterstützt werden.

Zumindest sollten Sie jedes Mal, wenn Sie eine neue Peerklasse definieren, die Methode [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getclassnamecore) implementieren, wie im nächsten Beispiel gezeigt.

<span codelanguage="CSharp"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>protected override string GetClassNameCore()
{
    return &quot;NumericUpDown&quot;;
}</code></pre></td>
</tr>
</tbody>
</table>

**Hinweis**  Sie sollten die Zeichenfolgen als Konstanten speichern und nicht direkt in der Methode. Die Entscheidung liegt jedoch bei Ihnen. Für [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getclassnamecore) müssen Sie diese Zeichenfolge nicht lokalisieren. Jedes Mal, wenn ein Benutzeroberflächenautomatisierungs-Client eine lokalisierte Zeichenfolge benötigt, wird die **LocalizedControlType**-Eigenschaft verwendet, nicht **ClassName**.

 

### <span id="GetAutomationControlType"></span><span id="getautomationcontroltype"></span><span id="GETAUTOMATIONCONTROLTYPE"></span>GetAutomationControlType

Einige Hilfstechnologien verwenden den Wert [**GetAutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209185_getautomationcontroltype) direkt, wenn sie Merkmale der Elemente eines Benutzeroberflächenautomatisierungs-Baums angeben, um zusätzliche Informationen über das **Name**-Element der Benutzeroberflächenautomatisierung hinaus bereitzustellen. Wenn sich Ihr Steuerelement erheblich von dem Steuerelement unterscheidet, von dem Sie ableiten, und Sie einen anderen Steuerelementtyp melden möchten als den, der durch die vom Steuerelement verwendete Basispeerklasse gemeldet wird, müssen Sie einen Peer implementieren und in Ihrer Implementierung des Peers [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getautomationcontroltypecore) überschreiben. Dies ist besonders wichtig, wenn Sie von einer allgemeinen Basisklasse wie [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) oder [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) ableiten, in der der Peer keine genauen Informationen über den Steuerelementtyp liefert.

Ihre Implementierung von [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getautomationcontroltypecore) beschreibt Ihr Steuerelement, indem ein [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182)-Wert zurückgegeben wird. Obwohl Sie **AutomationControlType.Custom** zurückgeben können, sollten Sie einen der spezifischeren Steuerelementtypen zurückgeben, wenn dieser die Hauptszenarien Ihres Steuerelements genauer beschreibt. Im Folgenden finden Sie ein Beispiel hierfür.

<span codelanguage="CSharp"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}</code></pre></td>
</tr>
</tbody>
</table>

**Hinweis**  Nur wenn Sie [**AutomationControlType.Custom**](https://msdn.microsoft.com/library/windows/apps/BR209182) angeben, müssen Sie [**GetLocalizedControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getlocalizedcontroltypecore) implementieren, um für Clients einen **LocalizedControlType**-Eigenschaftswert bereitzustellen. Die allgemeine Benutzeroberflächenautomatisierungs-Infrastruktur stellt übersetzte Zeichenfolgen für jeden möglichen **AutomationControlType**-Wert mit Ausnahme von **AutomationControlType.Custom** bereit.

 

### <span id="GetPattern_and_GetPatternCore"></span><span id="getpattern_and_getpatterncore"></span><span id="GETPATTERN_AND_GETPATTERNCORE"></span>GetPattern und GetPatternCore

Die Peerimplementierung von [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpatterncore) gibt das Objekt zurück, von dem das im Eingabeparameter angeforderte Muster unterstützt wird. Dabei ruft ein Benutzeroberflächenautomatisierungs-Client eine Methode auf, die an die [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpattern)-Methode des Anbieters weitergeleitet wird, und gibt einen [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496)-Enumerationswert an, mit dem das angeforderte Muster benannt wird. Ihre Überschreibung von **GetPatternCore** sollte das Objekt zurückgeben, mit dem das angegebene Muster implementiert wird. Bei diesem Objekt handelt es sich um den Peer selbst, da von diesem bei jeder Meldung, dass er ein Muster unterstützt, die entsprechende Musterschnittstelle implementiert werden sollte. Wenn Ihr Peer über keine benutzerdefinierte Implementierung eines Musters verfügt, Sie jedoch wissen, dass das Muster von der Basis des Peers implementiert wird, können Sie die Implementierung des Basistyps von **GetPatternCore** über **GetPatternCore** aufrufen. Das **GetPatternCore** eines Peers sollte **null** zurückgeben, wenn ein Muster nicht vom Peer unterstützt wird. Anstatt jedoch **null** direkt aus Ihrer Implementierung zurückzugeben, würden Sie gewöhnlich den Aufruf an die Basisimplementierung verwenden, um für jedes nicht unterstützte Muster **null** zurückzugeben.

Wenn ein Muster unterstützt wird, kann die [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpatterncore)-Implementierung **this** oder **Me** zurückgeben. Es wird erwartet, dass der Benutzeroberflächenautomatisierungs-Client den [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpattern)-Rückgabewert in die angeforderte Musterschnittstelle umwandelt, wenn diese nicht **null** ist.

Wenn eine Peerklasse von einem anderen Peer erbt und die notwendige Unterstützung und das Melden des Musters bereits von der Basisklasse behandelt wird, ist die Implementierung von [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpatterncore) nicht notwendig. Wenn Sie beispielsweise ein Bereichssteuerelement implementieren, das von [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863) abgeleitet wird, und Ihr Peer von [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) abgeleitet wird, gibt dieser Peer sich selbst für [**PatternInterface.RangeValue**](https://msdn.microsoft.com/library/windows/apps/BR242496) zurück und besitzt funktionierende Implementierungen der [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590)-Schnittstelle, die das Muster unterstützt.

Obwohl dies nicht der tatsächliche Code ist, kommt dieses Beispiel der Implementierung von [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpatterncore) sehr nahe, die bereits in [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) vorhanden ist.

<span codelanguage="CSharp"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    return base.GetPattern(patternInterface);
}</code></pre></td>
</tr>
</tbody>
</table>

Wenn Sie einen Peer implementieren, der nicht die gesamte Unterstützung bietet, die Sie von einer Basispeerklasse benötigen, oder wenn Sie die Gruppe der Muster, die von der Basis geerbt und von Ihrem Peer unterstützt werden, ändern oder ergänzen möchten, sollten Sie [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpatterncore) überschreiben, damit Benutzeroberflächenautomatisierungs-Clients die Muster verwenden können.

Unter [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225) finden Sie eine Liste der Anbietermuster, die in der UWP-Implementierung der Benutzeroberflächenautomatisierungs-Unterstützung verfügbar sind. Jedes solche Muster hat einen entsprechenden Wert der [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496)-Enumeration. Dies ist die Art, wie Benutzeroberflächenautomatisierungs-Clients das Muster in einem [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpattern)-Aufruf anfordern.

Ein Peer kann melden, dass er mehrere Muster unterstützt. In diesem Fall sollte die Überschreibung eine Rückgabepfadlogik für jeden unterstützten [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496)-Wert enthalten und den Peer bei jeder Übereinstimmung zurückgeben. Es wird erwartet, dass der Aufrufer jeweils nur eine Schnittstelle anfordert und dass der Aufrufer entscheiden kann, ob eine Umwandlung für die erwartete Schnittstelle durchgeführt wird.

Im Folgenden finden Sie ein Beispiel für eine [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpatterncore)-Überschreibung für einen benutzerdefinierten Peer. Der Peer meldet, dass die beiden Muster [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) und [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653) unterstützt werden. Bei dem hier verwendeten Steuerelement handelt es sich um ein Steuerelement zum Anzeigen von Medien, das als Vollbild (Umschaltmodus) dargestellt werden kann und über eine Fortschrittsleiste verfügt, auf der Benutzer eine Position (das Bereichssteuerelement) auswählen können. Dieser Code stammt aus dem [XAML-Beispiel für Barrierefreiheit](http://go.microsoft.com/fwlink/p/?linkid=238570).

<span codelanguage="CSharp"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    else if (patternInterface == PatternInterface.Toggle)
    {
        return this;
    }
    return null;
}</code></pre></td>
</tr>
</tbody>
</table>

### <span id="Forwarding_patterns_from_subelements"></span><span id="forwarding_patterns_from_subelements"></span><span id="FORWARDING_PATTERNS_FROM_SUBELEMENTS"></span>Weiterleiten von Mustern aus Unterelementen

Eine [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpatterncore)-Methodenimplementierung kann auch ein Unterelement oder einen Teil als Musteranbieter für den Host angeben. Dieses Beispiel zeigt, wie mit [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) die Behandlung des Bildlaufmusters an den Peer des internen [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/BR209527)-Steuerelements übergeben wird. Um ein Unterelement für die Musterbehandlung anzugeben, ruft dieser Code das Unterelementobjekt ab, erstellt mit der [**FrameworkElement.CreatePeerForElement**](https://msdn.microsoft.com/library/windows/apps/BR242472_createpeerforelement)-Methode einen Peer für das Unterelement und gibt den neuen Peer zurück.

<span codelanguage="CSharp"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.Scroll)
    {
        ItemsControl owner = (ItemsControl) base.Owner;
        UIElement itemsHost = owner.ItemsHost;
        ScrollViewer element = null;
        while (itemsHost != owner)
        {
            itemsHost = VisualTreeHelper.GetParent(itemsHost) as UIElement;
            element = itemsHost as ScrollViewer;
            if (element != null)
            {
                break;
            }
        }
        if (element != null)
        {
            AutomationPeer peer = FrameworkElementAutomationPeer.CreatePeerForElement(element);
            if ((peer != null) &amp;&amp; (peer is IScrollProvider))
            {
                return (IScrollProvider) peer;
            }
        }
    }
    return base.GetPatternCore(patternInterface);
}</code></pre></td>
</tr>
</tbody>
</table>

### <span id="Other_Core_methods"></span><span id="other_core_methods"></span><span id="OTHER_CORE_METHODS"></span>Weitere Core-Methoden

Ihr Steuerelement muss vielleicht Tastaturentsprechungen für primäre Szenarien unterstützen. Weitere Informationen darüber, warum dies erforderlich sein kann, finden Sie unter [Barrierefreiheit für den Tastaturzugriff](keyboard-accessibility.md). Die Implementierung der Schlüsselunterstützung ist zwingend Teil des Codes für das Steuerelement und nicht des Codes für den Peer, da es sich hierbei um einen Teil der Logik von Steuerelementen handelt. Ihre Peerklasse sollte die [**GetAcceleratorKeyCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getacceleratorkeycore)- und [**GetAccessKeyCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getaccesskeycore)-Methoden überschreiben, um den Benutzeroberflächenautomatisierungs-Clients zu melden, welche Schlüssel verwendet werden. Bedenken Sie, dass Zeichenfolgen, in denen Schlüsselinformationen gemeldet werden, eventuell lokalisiert werden müssen und daher aus Ressourcen stammen sollten, nicht aus hartcodierten Zeichenfolgen.

Wenn Sie einen Peer für eine Klasse bereitstellen, die eine Sammlung unterstützt, sollte am besten sowohl von Funktionsklassen als auch von Peerklassen abgeleitet werden, die bereits diese Art von Sammlungsunterstützung besitzen. Wenn dies nicht möglich ist, müssen Peers für Steuerelemente, die untergeordnete Sammlungen verwalten, möglicherweise die sammlungsbezogene Peermethode [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getchildrencore) überschreiben, um dem Benutzeroberflächenautomatisierungs-Baum die Beziehungen zwischen übergeordneten und untergeordneten Elementen korrekt zu melden.

Implementieren Sie die Methoden [**IsContentElementCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_iscontentelementcore) und [**IsControlElementCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_iscontrolelementcore), um anzugeben, ob Ihr Steuerelement Dateninhalte enthält und/oder eine interaktive Rolle auf der Benutzeroberfläche ausübt. Standardmäßig geben beide Methoden **true** zurück. Diese Einstellungen verbessern die Benutzerfreundlichkeit von Hilfstechnologien wie Bildschirmleseprogrammen, die diese Methoden möglicherweise zum Filtern der Automatisierungsstruktur verwenden. Wenn Ihre [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpatterncore)-Methode die Musterbehandlung an einen Unterelementpeer überträgt, kann die **IsControlElementCore**-Methode des Unterelementpeers **false** zurückgeben, um den Unterelementpeer in der Automatisierungsstruktur auszublenden.

Einige Steuerelemente stellen möglicherweise Unterstützung für Bezeichnungsszenarien bereit, in denen ein Teil mit einer Textbezeichnung Informationen für einen Teil ohne Text liefert, oder in denen sich ein Steuerelement in einer bekannten Bezeichnungsbeziehung zu einem anderen Steuerelement in der Benutzeroberfläche befinden soll. Wenn es möglich ist, ein nützliches klassenbasiertes Verhalten bereitzustellen, können Sie [**GetLabeledByCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getlabeledbycore) überschreiben, um dieses Verhalten bereitzustellen.

[**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getboundingrectanglecore) und [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getclickablepointcore) werden hauptsächlich für automatisierte Testszenarien verwendet. Wenn Sie automatisierte Tests für Ihr Steuerelement unterstützen möchten, sollten Sie diese Methoden überschreiben. Dies kann für Bereichssteuerelemente nützlich sein, bei denen Sie nicht nur einen einzelnen Punkt vorschlagen können, weil sich abhängig von der Klickposition des Benutzers im Koordinatenbereich jeweils eine andere Auswirkung auf einen Bereich ergibt. Beispielsweise überschreibt der standardmäßige [**ScrollBar**](https://msdn.microsoft.com/library/windows/apps/BR209745)-Automatisierungspeer **GetClickablePointCore**, um einen „keine Zahl“-Wert des Typs [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) zurückzugeben.

[**GetLiveSettingCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getlivesettingcore) beeinflusst den Steuerelementstandardwert für den **LiveSetting**-Wert für die Benutzeroberflächenautomatisierung. Sie sollten dies überschreiben, wenn das Steuerelement einen anderen Wert als [**AutomationLiveSetting.Off**](https://msdn.microsoft.com/library/windows/apps/JJ191519) zurückgeben soll. Weitere Informationen zu **LiveSetting** finden Sie unter [**AutomationProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516).

Sie können [**GetOrientationCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getorientationcore) überschreiben, wenn das Steuerelement über eine Eigenschaft mit festlegbarer Ausrichtung verfügt, die [**AutomationOrientation**](https://msdn.microsoft.com/library/windows/apps/BR209184) zugeordnet werden kann. Dies ist mit den Klassen [**ScrollBarAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242522) und [**SliderAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242546) möglich.

### <span id="Base_implementation_in_FrameworkElementAutomationPeer"></span><span id="base_implementation_in_frameworkelementautomationpeer"></span><span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"></span>Basisimplementierung in FrameworkElementAutomationPeer

Die Basisimplementierung von [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) bietet einige Informationen zur Benutzeroberflächenautomatisierung, die von verschiedenen, auf der Frameworkebene definierten Layout- und Verhaltenseigenschaften interpretiert werden können.

-   [**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getboundingrectanglecore): Gibt eine [**Rect**](https://msdn.microsoft.com/library/windows/apps/BR225994)-Struktur auf der Basis der bekannten Layouteigenschaften zurück. Gibt einen 0-Wert **Rect** zurück, wenn [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/BR209185_isoffscreen) **true** ist.
-   [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getclickablepointcore): Gibt eine [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870)-Struktur basierend auf den bekannten Layouteigenschaften zurück, solange ein **BoundingRectangle** ungleich Null vorhanden ist.
-   [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getnamecore): Hier können umfassendere Verhaltensweisen zusammengefasst werden; siehe [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getnamecore). Grundsätzlich wird versucht, eine Zeichenfolgenkonvertierung für alle bekannten Inhalte einer [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365)-Klasse oder verwandter Klassen auszuführen, die über Inhalte verfügen. Wenn ein Wert für [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) vorhanden ist, wird darüber hinaus der **Name**-Wert dieses Elements als **Name** verwendet.
-   [**HasKeyboardFocusCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_haskeyboardfocuscore): Ausgewertet basierend auf den Eigenschaften [**FocusState**](https://msdn.microsoft.com/library/windows/apps/BR209390_focusstate) und [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209390_isenabled) des Besitzers. Elemente, die keine Steuerelemente sind, geben stets **false** zurück.
-   [**IsEnabledCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_isenabledcore): Ausgewertet basierend auf der Eigenschaft [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209390_isenabled) des Besitzers, wenn diese ein [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) ist. Elemente, die keine Steuerelemente sind, geben stets **true** zurück. Dies bedeutet nicht, dass der Besitzer im herkömmlichen Sinn einer Interaktion aktiviert wird, sondern dass der Peer aktiviert wird, obwohl der Besitzer nicht über die Eigenschaft **IsEnabled** verfügt.
-   [**IsKeyboardFocusableCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_iskeyboardfocusablecore): Gibt **true** zurück, wenn der Besitzer ein [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) ist, andernfalls **false**.
-   [**IsOffscreenCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_isoffscreencore): [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208911_visibility) von [**Hidden**](https://msdn.microsoft.com/library/windows/apps/BR209006) für das Besitzerelement oder eines seiner übergeordneten Elemente entspricht dem Wert **true** für [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/BR209185_isoffscreen). Ausnahme: ein [**Popup**](https://msdn.microsoft.com/library/windows/apps/BR227842)-Objekt kann sichtbar sein, auch wenn dies nicht auf dessen übergeordneten Elemente zutrifft.
-   [**SetFocusCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_setfocuscore): Ruft [**Focus**](https://msdn.microsoft.com/library/windows/apps/BR209390_focus) auf.
-   [**GetParent**](https://msdn.microsoft.com/library/windows/apps/BR209185_getparent): Ruft [**FrameworkElement.Parent**](https://msdn.microsoft.com/library/windows/apps/BR208739) vom Besitzer auf und schlägt den entsprechenden Peer nach. Da es sich hier nicht um ein Überschreibungspaar mit einer „Core“-Methode handelt, können Sie dieses Verhalten nicht ändern.

**Hinweis**  Standard-UWP-Peers implementieren ein Verhalten mithilfe von systemeigenem Code, der die UWP implementiert, und nicht notwendigerweise mithilfe von tatsächlichem UWP-Code. Sie können den Code oder die Logik der Implementierung nicht mittels Common Language Runtime (CLR)-Spiegelung oder anderer Verfahren anzeigen. Außerdem sehen Sie keine einzelnen Referenzseiten in der Referenz für unterklassenspezifische Überschreibungen von Basispeerverhalten. Beispielsweise gibt es möglicherweise zusätzliche Verhaltensweisen für das [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getnamecore)-Element von [**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550), die nicht auf der **AutomationPeer.GetNameCore**-Referenzseite beschrieben werden, und es ist keine Referenzseite für **TextBoxAutomationPeer.GetNameCore** vorhanden. Es gibt noch nicht einmal eine **TextBoxAutomationPeer.GetNameCore**-Referenzseite. Lesen Sie stattdessen das Referenzthema für die unmittelbarste Peerklasse, und suchen Sie im Abschnitt für Anmerkungen nach Implementierungshinweisen.

 

<span id="Peers_and_AutomationProperties"></span><span id="peers_and_automationproperties"></span><span id="PEERS_AND_AUTOMATIONPROPERTIES"></span>Peers und AutomationProperties
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Ihr Automatisierungspeer sollte entsprechende Standardwerte für die Informationen bereitstellen, die sich auf die Barrierefreiheit des Steuerelements beziehen. Beachten Sie, dass jeder App-Code, der das Steuerelement verwendet, einen Teil dieses Verhaltens überschreiben kann, indem Werte für die angefügte [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) Eigenschaft in die Steuerelementinstanzen eingeschlossen werden. Aufrufer können dies entweder für die Standardsteuerelemente oder für benutzerdefinierte Steuerelemente ausführen. Mithilfe des folgenden XAML wird beispielsweise eine Schaltfläche erstellt, die über zwei angepasste Benutzeroberflächenautomatisierungs-Eigenschaften verfügt: `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

Weitere Informationen zu angefügten [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081)-Eigenschaften finden Sie unter [Grundlegende Barrierefreiheitsinformationen](basic-accessibility-information.md).

Einige [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)-Methoden sind aufgrund des allgemeinen Vertrags vorhanden, der festlegt, wie Benutzeroberflächenautomatisierungs-Anbieter Informationen melden sollen. Jedoch werden diese Methoden in der Regel nicht in Steuerelementpeers implementiert. Dies liegt daran, dass erwartet wird, dass die [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081)-Werte, die auf den App-Code angewendet werden, der die Steuerelemente auf einer bestimmten Benutzeroberfläche verwendet, diese Informationen liefern. Beispielweise würde die Mehrzahl der Apps die Bezeichnungsbeziehung zwischen zwei verschiedenen Steuerelementen in der Benutzeroberfläche definieren, indem sie den Wert [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) anwendet. [
            **LabeledByCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getlabeledbycore) ist jedoch in bestimmten Peers implementiert, die Daten- oder Objektbeziehungen in einem Steuerelement darstellen, beispielsweise die Verwendung einer Headerkomponente zum Bezeichnen einer Datenfeldkomponente, das Bezeichnen von Objekten mit ihren Containern oder ähnliche Szenarien.

<span id="_Implementing_patterns"></span><span id="_implementing_patterns"></span><span id="_IMPLEMENTING_PATTERNS"></span> Implementieren von Mustern
-------------------------------------------------------------------------------------------------------------------------------------------------

Als Nächstes wird beschrieben, wie ein Peer für ein Steuerelement geschrieben wird, mit dem durch die Implementierung der Steuerelement-Musterschnittstelle für Erweitern/Reduzieren das Verhalten zum Erweitern/Reduzieren implementiert wird. Der Peer sollte die Barrierefreiheit für das Erweitern/Reduzieren ermöglichen, indem er jedes Mal, wenn [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/BR209185_getpattern) mit einem Wert von [**PatternInterface.ExpandCollapse**](https://msdn.microsoft.com/library/windows/apps/BR242496) aufgerufen wird, sich selbst zurückgibt. Anschließend sollte der Peer die Anbieterschnittstelle für dieses Muster ([**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/desktop/Ee671242)) erben und Implementierungen für alle Member dieser Anbieterschnittstelle bereitstellen. In diesem Fall muss die Schnittstelle drei Member überschreiben: [**Expand**](https://msdn.microsoft.com/library/windows/apps/BR242570), [**Collapse**](https://msdn.microsoft.com/library/windows/apps/BR242569) und [**ExpandCollapseState**](https://msdn.microsoft.com/library/windows/apps/BR242570collapsestate).

Es ist hilfreich, die Barrierefreiheit im API-Entwurf der Klasse selbst vorauszuplanen. Stellen Sie für ein Verhalten, das möglicherweise durch typische Benutzerinteraktionen mit der UI oder ein Automatisierungsanbietermuster angefordert wird, eine einzelne Methode bereit, die entweder als Benutzeroberflächenreaktion oder durch das Automatisierungsmuster aufgerufen werden kann. Wenn Ihr Steuerelement beispielsweise Schaltflächenteile mit verknüpften Ereignishandlern, die das Steuerelement erweitern oder reduzieren können, sowie Tastaturentsprechungen für diese Aktionen enthält, sollten diese Ereignishandler die gleiche Methode aufrufen, die aus den [**Expand**](https://msdn.microsoft.com/library/windows/apps/BR242570)- oder [**Collapse**](https://msdn.microsoft.com/library/windows/apps/BR242569)-Implementierungen für [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/desktop/Ee671242) im Peer aufgerufen wird. Es kann auch nützlich sein, mit einer allgemeinen Logikmethode sicherzustellen, dass die visuellen Zustände Ihres Steuerelements aktualisiert werden, um den logischen Zustand einheitlich anzuzeigen, unabhängig davon, wie das Verhalten aufgerufen wurde.

Bei einer typischen Implementierung wird von den Anbieter-APIs zuerst der [**Owner**](https://msdn.microsoft.com/library/windows/apps/BR242472_owner) aufgerufen, um Zugriff zur Laufzeit auf die Steuerelementinstanz zu erhalten. Danach können die erforderlichen Verhaltensmethoden für dieses Objekt aufgerufen werden.

<span codelanguage="CSharp"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner) 
    { 
         ownerIndexCard = owner;
    } 
}</code></pre></td>
</tr>
</tbody>
</table>

Eine alternative Implementierungsmöglichkeit besteht darin, dass das Steuerelement selbst auf seinen Peer verweist. Dies ist ein häufig verwendetes Muster, wenn Automatisierungsereignisse über das Steuerelement ausgelöst werden, da die [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/BR209185_raiseautomationevent)-Methode eine Peermethode ist.

<span id="UI_Automation_events"></span><span id="ui_automation_events"></span><span id="UI_AUTOMATION_EVENTS"></span>Benutzeroberflächenautomatisierungs-Ereignisse
-----------------------------------------------------------------------------------------------------------------------------------------

Benutzeroberflächenautomatisierungs-Ereignisse werden in die folgenden Kategorien unterteilt.

| Ereignis            | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Eigenschaftsänderung  | Wird ausgelöst, wenn eine Eigenschaft eines Benutzeroberflächenautomatisierungs-Elements oder Steuerelementmusters geändert wird. Wenn ein Client beispielsweise das Steuerelement für das Kontrollkästchen einer App überwachen muss, kann dieser sich registrieren, um auf ein Eigenschaftsänderungsereignis im Hinblick auf die [**ToggleState**](https://msdn.microsoft.com/library/windows/apps/BR242653_togglestate)-Eigenschaft zu hören. Wenn das Steuerelement für das Kontrollkästchen aktiviert oder deaktiviert wird, wird das Ereignis vom Anbieter ausgelöst, und der Client kann die erforderliche Aktion ausführen.                                                                                                                                                                                  |
| Aktion für Element   | Wird ausgelöst, wenn durch eine Aktivität des Benutzers oder eine programmgesteuerte Aktivität eine Änderung der Benutzeroberfläche ausgelöst wird, beispielsweise wenn über das Muster **Invoke** auf eine Schaltfläche geklickt oder diese aufgerufen wird.                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Strukturänderung | Wird ausgelöst, wenn der Benutzeroberflächenautomatisierungs-Baum geändert wird. Die Struktur wird geändert, wenn neue Benutzeroberflächenelemente angezeigt, ausgeblendet oder vom Desktop entfernt werden.                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Globale Änderung    | Wird ausgelöst, wenn Aktionen ausgeführt werden, die den Client global betreffen, beispielsweise wenn der Fokus von einem Element auf ein anderes gewechselt wird oder wenn ein untergeordnetes Fenster geschlossen wird. Einige Ereignisse implizieren nicht unbedingt, dass sich der Status der Benutzeroberfläche geändert hat. Wenn der Benutzer beispielsweise zu einem Texteingabefeld navigiert und anschließend auf eine Schaltfläche klickt, um das Feld zu aktualisieren, wird auch dann ein [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683_textchanged)-Ereignis ausgelöst, wenn der Benutzer den Text nicht geändert hat. Bei der Verarbeitung eines Ereignisses ist es u. U. erforderlich, von einer Clientanwendung überprüfen zu lassen, ob Änderungen vorliegen, bevor Maßnahmen ergriffen werden. |

 

### <span id="AutomationEvents_identifiers"></span><span id="automationevents_identifiers"></span><span id="AUTOMATIONEVENTS_IDENTIFIERS"></span>AutomationEvents-Bezeichner

Benutzeroberflächenautomatisierungs-Ereignisse werden durch [**AutomationEvents**](https://msdn.microsoft.com/library/windows/apps/BR209183)-Werte identifiziert. Durch die Werte der Enumeration wird die Art des Ereignisses eindeutig identifiziert.

### <span id="Raising_events"></span><span id="raising_events"></span><span id="RAISING_EVENTS"></span>Auslösen von Ereignissen

Benutzeroberflächenautomatisierungs-Clients können Automatisierungsereignisse abonnieren. Im Automatisierungspeermodell müssen Peers für benutzerdefinierte Steuerelemente Änderungen am Zustand des Steuerelements, die für die Barrierefreiheit relevant sind, durch den Aufruf der [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/BR209185_raiseautomationevent)-Methode melden. Entsprechend sollten die Peers des benutzerdefinierten Steuerelements die [**RaisePropertyChangedEvent**](https://msdn.microsoft.com/library/windows/apps/BR209185_raisepropertychangedevent)-Methode aufrufen, wenn sich der Wert einer wichtigen Benutzeroberflächenautomatisierungs-Eigenschaft ändert.

Das folgende Codebeispiel zeigt, wie das Peerobjekt aus dem Steuerelement-Definitionscode abgerufen und eine Methode aufgerufen wird, um von diesem Peer aus ein Ereignis auszulösen. Zur Optimierung stellt der Code fest, ob es Listener für diesen Ereignistyp gibt. Wenn das Ereignis nur ausgelöst und das Peerobjekt nur erstellt wird, wenn Listener vorhanden sind, entsteht kein unnötiger Aufwand, und das Steuerelement bleibt reaktionsfähig.

<span codelanguage="CSharp"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>if (AutomationPeer.ListenerExists(AutomationEvents.PropertyChanged))
{
    NumericUpDownAutomationPeer peer =
        FrameworkElementAutomationPeer.FromElement(nudCtrl) as NumericUpDownAutomationPeer;
    if (peer != null)
    {
        peer.RaisePropertyChangedEvent(
            RangeValuePatternIdentifiers.ValueProperty,
            (double)oldValue,
            (double)newValue);
    }
}</code></pre></td>
</tr>
</tbody>
</table>

<span id="Peer_navigation"></span><span id="peer_navigation"></span><span id="PEER_NAVIGATION"></span>Peernavigation
---------------------------------------------------------------------------------------------------------------------

Nachdem ein Automatisierungspeer gefunden wurde, kann ein Benutzeroberflächenautomatisierungs-Client in der Peerstruktur einer App navigieren, indem die Methoden [**GetChildren**](https://msdn.microsoft.com/library/windows/apps/BR209185_getchildren) und [**GetParent**](https://msdn.microsoft.com/library/windows/apps/BR209185_getparent) des Peerobjekts aufgerufen werden. Die Navigation zwischen Benutzeroberflächenelementen in einem Steuerelement wird von der Implementierung der [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/BR209185_getchildrencore)-Methode des Peers unterstützt. Das Benutzeroberflächenautomatisierungs-System ruft diese Methode auf, um eine Struktur der in einem Steuerelement enthaltenen Unterelemente zu erstellen, beispielsweise von Listenelementen in einem Listenfeld. Die **GetChildrenCore**-Standardmethode in [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) durchläuft die visuelle Struktur der Elemente, um die Struktur der Automatisierungspeers zu erstellen. Benutzerdefinierte Steuerelemente können diese Methode überschreiben, um eine andere Darstellung der untergeordneten Elemente für Automatisierungsclients verfügbar zu machen. Dabei werden die Automatisierungspeers der Elemente zurückzugeben, die Informationen übermitteln oder Benutzerinteraktion erlauben.

<span id="Native_automation_support_for_text_patterns"></span><span id="native_automation_support_for_text_patterns"></span><span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"></span>Systemeigene Automatisierungsunterstützung für Textmuster
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Einige der Standardautomatisierungspeers für UWP-Apps bieten Unterstützung für Steuerelementmuster ([**PatternInterface.Text**](https://msdn.microsoft.com/library/windows/apps/BR242496)). Diese Unterstützung erfolgt jedoch über systemeigene Methoden, sodass die betroffenen Peers nicht die [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627)-Schnittstelle in der (verwalteten) Vererbung benachrichtigen. Wenn ein verwalteter oder nicht verwalteter Benutzeroberflächenautomatisierungs-Client den Peer nach Mustern abfragt, meldet dieser dennoch die Unterstützung des Textmusters und stellt Verhaltensweisen für Teile des Musters bereit, wenn die Client-APIs aufgerufen werden.

Wenn Sie von einem Textsteuerelement einer UWP-App ableiten möchten, können Sie auch einen benutzerdefinierten Peer erstellen, der von einem der textspezifischen Peers abgeleitet ist. Im Abschnitt "Anmerkungen" des Peers finden Sie weitere Informationen zur möglichen systemeigenen Unterstützung von Mustern. Der Zugriff auf das systemeigene Verhalten Ihres benutzerdefinierten Peers erfolgt durch den Aufruf der Basisimplementierung Ihrer verwalteten Anbieterschnittstellenimplementierungen. es ist aber schwierig, das Verhalten der Basisimplementierung zu ändern, da die systemeigenen Schnittstellen weder im Peer noch im Eigentümersteuerelement verfügbar gemacht werden. Sie sollten im Allgemeinen entweder die Basisimplementierungen wie bereitgestellt verwenden (durch Aufruf der Basis) oder die Funktionen vollständig im eigenen verwalteten Code ersetzen und die Basisimplementierung gar nicht aufrufen. Im letzten Fall handelt es sich um ein fortgeschrittenes Szenario. Sie müssen mit dem von Ihrem Steuerelement verwendeten Textdienstframework vertraut sein, um die Anforderungen hinsichtlich der Barrierefreiheit zu unterstützen, wenn Sie dieses Framework verwenden.

<span id="AutomationProperties.AccessibilityView"></span><span id="automationproperties.accessibilityview"></span><span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"></span>AutomationProperties.AccessibilityView
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Zusätzlich zum Bereitstellen eines benutzerdefinierten Peers können Sie auch die Strukturansichtdarstellung für beliebige Steuerelementinstanzen anpassen, indem Sie im XAML-Code [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/BR209081_accessibilityview) festlegen. Dies wird zwar nicht als Teil einer Peerklasse implementiert, wird hier jedoch erwähnt, weil es für die allgemeine Barrierefreiheitsunterstützung für benutzerdefinierte Steuerelemente oder für von Ihnen angepasste Vorlagen relevant ist.

Das Hauptszenario für die Verwendung von [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/BR209081_accessibilityview) ist das absichtliche Auslassen bestimmter Steuerelemente in einer Vorlage aus den Benutzeroberflächenautomatisierungs-Ansichten, weil sie keinen nennenswerten Beitrag zur Barrierefreiheitsansicht des gesamten Steuerelements leisten. Um dies zu verhindern, legen Sie **AutomationProperties.AccessibilityView** auf „Raw“ fest. Weitere Informationen finden Sie unter [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/BR209081_accessibilityview).

<span id="Throwing_exceptions_from_automation_peers"></span><span id="throwing_exceptions_from_automation_peers"></span><span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"></span>Auslösen von Ausnahmen über Automatisierungspeers
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Die APIs, die Sie für Ihre Automatisierungspeerunterstützung implementieren, dürfen Ausnahmen auslösen. Es wird erwartet, dass lauschende Benutzeroberflächenautomatisierungs-Clients stabil genug sind, um nach dem Auslösen der meisten Ausnahmen weiter zu funktionieren. Aller Wahrscheinlichkeit nach betrachtet der Listener eine Automatisierungsgesamtstruktur, die neben Ihrer eigenen App auch andere Apps enthält. Das Clientdesign darf nicht zum Ausfall des gesamten Clients führen, wenn beim Aufrufen der APIs in einem Bereich der Struktur eine peerbasierte Ausnahme ausgelöst wird.

Parameter, die an Ihren Peer übergeben werden, können die Eingabe überprüfen und beispielsweise [**ArgumentNullException**](T:System.ArgumentNullException) auslösen, wenn **null** übergeben wurde und dies für Ihre Implementierung kein gültiger Wert ist. Wenn jedoch nachfolgende Vorgänge von Ihrem Peer ausgeführt werden, denken Sie daran, dass die Interaktionen des Peers mit dem hostenden Steuerelement einen gewissen asynchronen Charakter haben können. Durch Aktionen eines Peers wird nicht zwangsläufig der Benutzeroberflächenthread im Steuerelement blockiert (dies sollte vermutlich auch nicht geschehen). Daher sind Situationen denkbar, in denen ein Objekt bei der Erstellung des Peers oder beim ersten Aufruf einer Automatisierungspeermethode verfügbar war oder bestimmte Eigenschaften hatte, während sich der Zustand des Steuerelements in der Zwischenzeit geändert hat. Für diese Fälle gibt es zwei spezielle Ausnahmen, die ein Anbieter auslösen kann:

-   Lösen Sie [**ElementNotAvailableException**](https://msdn.microsoft.com/library/windows/apps/Hh673741) aus, wenn Sie basierend auf den ursprünglich an Ihre APIs übergebenen Informationen nicht auf den Besitzer des Peers oder ein verwandtes Peerelement zugreifen können. Beispielsweise gibt es möglicherweise einen Peer, der seine Methoden auszuführen versucht, dessen Besitzer jedoch in der Zwischenzeit aus der Benutzeroberfläche entfernt wurde. Dies ist beispielsweise der Fall, wenn ein modales Dialogfeld geschlossen wird. Bei anderen als .NET-Clients entspricht dies [**UIA\_E\_ELEMENTNOTAVAILABLE**](https://msdn.microsoft.com/library/windows/desktop/Ee671218).
-   Lösen Sie [**ElementNotEnabledException**](https://msdn.microsoft.com/library/windows/apps/Hh673748) aus, wenn zwar noch ein Besitzer vorhanden ist, dieser sich jedoch in einem Modus wie [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209390_isenabled)`=`**false** befindet, der einen Teil der spezifischen programmgesteuerten Änderungen blockiert, die der Peer auszuführen versucht. Bei anderen als .NET-Clients entspricht dies [**UIA\_E\_ELEMENTNOTENABLED**](https://msdn.microsoft.com/library/windows/desktop/Ee671218).

Darüber hinaus sollten Peers mit Ausnahmen, die sie über die Peerunterstützung auslösen, vergleichsweise konservativ umgehen. Die meisten Clients können Ausnahmen von Peers nicht verarbeiten und wandeln diese in Wahlmöglichkeiten mit ausführbaren Aktionen um, die Benutzer bei Interaktionen mit dem Client auswählen können. Manchmal ist es also eine bessere Strategie, keinen Vorgang auszuführen und Ausnahmen ohne erneutes Auslösen in Ihren Peerimplementierungen abzufangen, als bei jeder fehlgeschlagenen Aktion des Peers Ausnahmen auszulösen. Berücksichtigen Sie auch, dass die meisten Benutzeroberflächenautomatisierungs-Clients nicht in verwaltetem Code geschrieben werden. Die meisten werden in COM geschrieben und überprüfen lediglich **S\_OK** in **HRESULT**, wenn sie eine Benutzeroberflächenautomatisierungs-Clientmethode aufrufen, die auf Ihren Peer zugreift.

<span id="related_topics"></span>Verwandte Themen
-----------------------------------------------

* [Barrierefreiheit](accessibility.md)
* [XAML-Beispiel für Barrierefreiheit](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472)
* [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)
* [**OnCreateAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR208911_oncreateautomationpeer)
* [Steuerelementmuster und Schnittstellen](control-patterns-and-interfaces.md)
 

 





<!--HONumber=Mar16_HO3-->


