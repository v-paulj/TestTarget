---
Description: Windows 10 und neue Entwicklungstools stellen Werkzeuge, Features und Umgebungen zur Verfügung, die durch die neue universelle Windows-Plattform (UWP) unterstützt werden.
title: Neuigkeiten für Entwickler in Windows 10, RTM – Juli 2015
---

# Neuigkeiten für Entwickler in Windows 10, RTM: Juli 2015


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Windows 10 und neue Entwicklungstools stellen Werkzeuge, Features und Umgebungen zur Verfügung, die durch die neue universelle Windows-Plattform (UWP) unterstützt werden. Nach der [Installation der Tools und des SDKs](https://dev.windows.com/downloads) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](https://msdn.microsoft.com/library/windows/apps/bg124288) oder lesen, wie Sie Ihren [vorhandenen App-Code unter Windows verwenden](https://msdn.microsoft.com/library/windows/apps/mt238321) können.

Im Folgenden werden die neuen Features in Windows 10, RTM einzeln erläutert.

## Adaptive Layouts


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Mehrere Ansichten für maßgeschneiderte Inhalte</td>
<td align="left">XAML bietet neue Unterstützung für die Definition von maßgeschneiderten Ansichten (XAML-Dateien), die eine gemeinsame Codedatei verwenden. Dies erleichtert Ihnen das Erstellen und Verwalten verschiedener Ansichten, die auf eine bestimmte Gerätefamilie oder ein Szenario zugeschnitten sind. Erstellen Sie mehrere Ansichten, wenn Ihre App eine Benutzeroberfläche mit unterschiedlichem Inhalt, Layout oder Navigationsmodellen enthält, die sich für verschiedene Szenarien erheblich unterscheiden. Beispielsweise können Sie für Ihre mobile App ein [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241)-Element mit einer für einhändige Nutzung optimierten Navigation verwenden, für Ihre Desktop-App aber ein [<strong>SplitView</strong>](https://msdn.microsoft.com/library/windows/apps/dn864360)-Element mit einem für die Maus optimierten Navigationsmenü.</td>
</tr>
<tr class="even">
<td align="left">StateTriggers</td>
<td align="left">Mit dem neuen Feature [<strong>VisualState.StateTriggers</strong>](https://msdn.microsoft.com/library/windows/apps/dn890396) können Sie Eigenschaften basierend auf der Höhe/Breite des Fensters oder basierend auf einem benutzerdefinierten Auslöser bedingt festlegen. Bisher mussten Sie [<strong>SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br209055)-Fensterereignisse im Code behandeln und [<strong>VisualStateManager.GoToState</strong>](https://msdn.microsoft.com/library/windows/apps/br209025) aufrufen.</td>
</tr>
<tr class="odd">
<td align="left">Setter</td>
<td align="left"><p>Mit der neuen [<strong>VisualState.Setters</strong>](https://msdn.microsoft.com/library/windows/apps/dn890395)-Syntax können Sie ein vereinfachtes Markup zum Definieren von Eigenschaftsänderungen in [<strong>VisualStateManager</strong>](https://msdn.microsoft.com/library/windows/apps/br209021) verwenden. Bisher mussten Sie ein Storyboard verwenden und Animationen erstellen, um Eigenschaftsänderungen anzuwenden, z. B. das Ändern der Ausrichtung eines [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635)-Elements von horizontal in vertikal. In universellen Windows-Apps können Sie diese einfachere Setter-Syntax verwenden:</p>
<p><code>&lt;setter target=&quot;stackPanel1.Orientation&quot; value=&quot;Vertical&quot; /&gt;</code></p></td>
</tr>
</tbody>
</table>

 

## XAML-Features


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Kompilierte Datenbindungen (x:Bind)</td>
<td align="left"><p>In universellen Windows-Apps können Sie den neuen compilerbasierten Bindungsmechanismus verwenden, der durch die x:Bind-Eigenschaft aktiviert wird. Compilerbasierte Bindungen werden stark typisiert und zur Kompilierzeit verarbeitet, was schneller ist und Kompilierzeitfehler meldet, wenn Bindungstypen nicht übereinstimmen. Da Bindungen in kompilierten App-Code übersetzt werden, können Sie jetzt Bindungen debuggen, indem Sie den Code in Visual Studio durchlaufen, um bestimmte Bindungsprobleme zu diagnostizieren. Sie können zum Binden an eine Methode „x:Bind“ auch wie folgt verwenden:</p>
<p><code>&lt;textblock text=&quot;{x:Bind Customer.Address.ToString()}&quot; /&gt;</code></p>
<p>Für typische Bindungsszenarien können Sie „x:Bind“ anstelle von „Binding“ verwenden und dadurch die Leistung und Wartbarkeit verbessern.</p></td>
</tr>
<tr class="even">
<td align="left">Deklaratives inkrementelles Rendering von Listen (x:Phase)</td>
<td align="left"><p>Mit dem neuen x:Phase-Attribut können Sie in universellen Windows-Apps inkrementelles Rendering oder Phasenrendering von Listen mit XAML anstelle von Code durchführen. Beim Verschieben von langen Listen mit komplexen Elementen ist Ihre App beim Rendern möglicherweise nicht schnell genug, um mit der Geschwindigkeit der Verschiebung mithalten zu können, was möglicherweise ein schlechtes Benutzererlebnis zur Folge hat. Bei Phasen-Rendering können Sie die Rendering-Priorität einzelner Elemente in einem Listenelement festlegen, damit in schnellen Rendering-Szenarios nur die wichtigsten Teile des Listenelements gerendert werden. Dies führt zu einem reibungsloseren Verschiebungserlebnis für den Benutzer.</p>
<p>In Windows 8.1 könnten Sie das [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914)-Ereignis behandeln und Code zum Rendern der Listenelemente in mehreren Phasen schreiben. In UWP-Apps können Sie Phasenrendering erreichen, indem Sie das x:Phase-Attribut deklarativ verwenden. In Verbindung mit kompilierten Bindungen mit X:Bind können Sie mit x:Phase einfach eine Rendering-Priorität für jedes gebundene Element eine Datenvorlage festlegen. Beim Verschieben basiert die Arbeit für das Rendern von Elementen auf phasenbasierten Zeitscheiben, was inkrementelles Rendern von Elementen ermöglicht.</p></td>
</tr>
<tr class="odd">
<td align="left">Verzögertes Laden von Elementen der Bedienoberfläche (x:DeferLoadingStrategy)</td>
<td align="left">In universellen Windows-Apps können Sie mithilfe der neuen Direktive „x:DeferLoadingStrategy“ festlegen, dass Teile Ihrer Benutzeroberfläche verzögert geladen werden; dies verbessert die Startleistung und verringert die Speicherauslastung Ihrer App. Wenn auf Ihrer App-Benutzeroberfläche beispielsweise ein Element für die Datenvalidierung nur bei der Eingabe falscher Daten angezeigt wird, können Sie das Laden dieses Elements verzögern, bis es benötigt wird. Die Elementobjekte werden dann nicht beim Laden der Seite erstellt, sondern erst, wenn ein Datenfehler vorliegt und sie der visuellen Struktur der Seite hinzugefügt werden müssen.</td>
</tr>
<tr class="even">
<td align="left">SplitView</td>
<td align="left">Das neue [<strong>SplitView</strong>](https://msdn.microsoft.com/library/windows/apps/dn864360)-Steuerelement ist eine einfache Möglichkeit zum Ein- und Ausblenden vorübergehend angezeigter Inhalte. Es wird häufig für Navigationsszenarien auf oberster Ebene wie dem „Hamburger-Menü“ verwendet, in denen der Navigationsinhalt ausgeblendet ist, und bei Bedarf durch eine Benutzeraktion eingeblendet wird.</td>
</tr>
<tr class="odd">
<td align="left">RelativePanel</td>
<td align="left">[<strong>RelativePanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn879546) ist ein neues Layoutpanel, in dem Sie untergeordnete Objekte in Bezug aufeinander oder in Bezug auf das übergeordnete Panel positionieren und ausrichten können. Beispielsweise können Sie angeben, dass Text immer auf der linken Seite des Panels positioniert und dass eine Schaltfläche immer unterhalb des Texts ausgerichtet werden soll. Verwenden Sie <strong>RelativePanel</strong> beim Erstellen von Benutzeroberflächen ohne ein klares lineares Muster, das ein [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635)- oder [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704)-Element erfordert.</td>
</tr>
<tr class="even">
<td align="left">CalendarView</td>
<td align="left">Das [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052)-Steuerelement vereinfacht das Anzeigen und Auswählen von Datumsangaben und Datumsbereichen mithilfe einer anpassbaren, monatlichen Ansicht. <strong>CalendarView</strong> unterstützt Features wie minimale, maximale und Ausfalldatumsangaben, um die auswählbaren Datumsangaben einzugrenzen. Sie können auch benutzerdefinierte Dichtebalken festlegen, die zum Anzeigen der allgemeinen „Belegung“ des Zeitplans an einem bestimmten Tag verwendet werden können.</td>
</tr>
<tr class="odd">
<td align="left">CalendarDatePicker</td>
<td align="left">[<strong>CalendarDatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn950083) ist ein Dropdownsteuerelement, das für die Auswahl eines einzelnen Datums aus einer [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052) optimiert ist, in der kontextbezogene Informationen wie der Wochentag oder die Belegung des Kalenders von Bedeutung sind. Es ist mit dem [<strong>DatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn298584)-Steuerelement vergleichbar, <strong>DatePicker</strong> ist jedoch für die Auswahl eines bekannten Datums optimiert, z. B. eines Geburtsdatums.</td>
</tr>
<tr class="even">
<td align="left">MediaTransportControls</td>
<td align="left">Mit der neuen [<strong>MediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/apps/dn831962)-Klasse können Sie die Transportsteuerelemente eines [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926)-Elements einfacher anpassen. In Windows 8.1 konnten Sie in <strong>MediaElement</strong> integrierte Transportsteuerelemente aktivieren oder Ihre eigenen Transportsteuerelemente erstellen, die <strong>MediaElement</strong>-Methoden aufriefen. Jetzt können Sie die integrierte Funktionalität von <strong>MediaTransportControls</strong> verwenden und das Erscheinungsbild trotzdem problemlos an Ihre App anpassen.</td>
</tr>
<tr class="odd">
<td align="left">Benachrichtigungen über Eigenschaftsänderungen</td>
<td align="left"><p>In universellen Windows-Apps können Sie auf Eigenschaftsänderungen von [<strong>DependencyObject</strong>](https://msdn.microsoft.com/library/windows/apps/br242356) lauschen, und zwar auch für Eigenschaften, die über keine entsprechenden Änderungsereignisse verfügen.</p>
<p>Die Benachrichtigung funktioniert wie ein Ereignis, wird aber als Rückruf eingeblendet. Der Rückruf verwendet ein Absenderargument wie einen Ereignishandler, aber verwendet kein Ereignisargument. Stattdessen wird nur der Eigenschaftenbezeichner weitergeleitet, um die Eigenschaft anzugeben. Mit diesen Informationen kann von Ihrer App ein einzelner Handler für mehrere Eigenschaftsbenachrichtigungen definiert werden. Weitere Informationen finden Sie unter [<strong>RegisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831902) und [<strong>UnregisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831910).</p></td>
</tr>
<tr class="even">
<td align="left">Karten</td>
<td align="left"><p>Die [<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse wurde aktualisiert, um 3D-Luftaufnahmen und Straßenansichten bereitzustellen. Diese neuen Features und die früheren Kartenfunktionen sind jetzt für universelle Windows-Apps verfügbar. Fügen Sie Ihrer App Kartenfunktionen mit den folgenden APIs hinzu:</p>
<ul>
<li>[<strong>Windows.UI.Xaml.Controls.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn610751)-Namespace – Anzeigen von Karten.</li>
<li>[<strong>Windows.Services.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace – Suchen nach Orten und Routen.</li>
</ul>
<p>Fordern Sie einen Schlüssel im [Bing Maps Developer Center](https://www.bingmapsportal.com/) an, um diese APIs noch heute in einer universellen Windows-App einsetzen zu können. Weitere Informationen finden Sie unter [How to authenticate a Maps app](https://msdn.microsoft.com/library/windows/apps/xaml/dn741528). Eine weitere Neuheit für Windows 10 besteht darin, dass PC- und Smartphone-Benutzer über die Einstellungs-App Offlinekarten herunterladen können. Sofern verfügbar, werden Offlinekarten vom [<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004)-Element zum Anzeigen von Karten genutzt, wenn kein Internetzugriff verfügbar ist.</p></td>
</tr>
<tr class="odd">
<td align="left">Zuordnung für Eingabeschaltfläche</td>
<td align="left">Die [<strong>Windows.UI.Xaml.Input.KeyRoutedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/hh943072)-Klasse enthält eine neue [<strong>OriginalKey</strong>](https://msdn.microsoft.com/library/windows/apps/dn960168)-Eigenschaft, die es Ihnen in Verbindung mit einer entsprechenden Aktualisierung von [<strong>Windows.System.VirtualKey</strong>](https://msdn.microsoft.com/library/windows/apps/br241812) ermöglicht, die ursprüngliche, nicht zugeordnete Eingabeschaltfläche abzurufen, die mit dem Tastatureingabeereignis verknüpft ist.</td>
</tr>
<tr class="even">
<td align="left">Freihandeingaben</td>
<td align="left"><p>Dank des [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements und der zugrunde liegenden [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011)-Klassen ist es jetzt einfacher, die stabile Freihandfunktion in Windows-Runtime-Apps mit C++, C# oder Visual Basic zu verwenden.</p>
<p>Das [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement definiert einen Überlagerungsbereich zum Zeichnen und Rendern letzter Striche. Die Funktionen dieses Steuerelements (Eingabe, Verarbeitung und Rendering) basieren auf den Klassen [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011), [<strong>InkStroke</strong>](https://msdn.microsoft.com/library/windows/apps/br208485), [<strong>InkRecognizer</strong>](https://msdn.microsoft.com/library/windows/apps/br208478) und [<strong>InkSynchronizer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903979).</p>
<p></p>
<div class="alert">
<strong>Wichtig</strong> Diese Klassen werden in Windows-Apps mit JavaScript nicht unterstützt.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## Aktualisierte XAML-Features


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">CommandBar- und AppBar-Updates</td>
<td align="left"><p>Das [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427)-Steuerelement und [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927)-Steuerelement wurden aktualisiert, um eine konsistente API sowie ein konsistentes Verhalten und Benutzererlebnis für UWP-Apps über alle Gerätefamilien hinweg zu gewährleisten.</p>
<p>Das [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427)-Steuerelement für universelle Windows-Apps wurde verbessert, um eine Übermenge von [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927)-Funktionen und flexiblere Verwendungsmöglichkeiten in Ihrer App bereitzustellen. Sie sollten <strong>CommandBar</strong> für alle neuen universellen Windows-Apps unter Windows 10 verwenden. In <strong>CommandBar</strong> unter Windows 8.1 konnten Sie nur Steuerelemente verwenden, von denen [<strong>ICommandBarElement</strong>](https://msdn.microsoft.com/library/windows/apps/dn251969), wie z. B. [<strong>AppBarButton</strong>](https://msdn.microsoft.com/library/windows/apps/dn279244), implementiert wurde. In universellen Windows-Apps können Sie jetzt neben <strong>AppBarButton</strong> benutzerdefinierte Inhalte auch in <strong>CommandBar</strong> einfügen.</p>
<p>Das [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927)-Steuerelement wurde aktualisiert, damit Sie Ihre Windows 8.1-Apps, die <strong>AppBar</strong> verwenden, einfacher zur universellen Windows-Plattform migrieren können. <strong>AppBar</strong> wurde zur Verwendung mit Vollbild-Apps entwickelt und wird mithilfe von Bewegungen vom Bildschirmrand aufgerufen. Durch Steuerelementupdates werden Probleme in Windows 10 behoben, z. B. bei Apps im Fenstermodus und wenn keine Wischbewegungen vom Bildschirmrand aus unterstützt werden.</p>
<p>Das ausgeblendete [<strong>AppBar.ClosedDisplayMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn633872)-Element, das zuvor nur in Windows Phone verfügbar war, wird jetzt auf allen Gerätefamilien unterstützt und ermöglicht die Auswahl zwischen verschiedenen Hinweisebenen für Befehle. [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) zeigt standardmäßig einen Mini-Hinweis an, der für Konsistenz sorgt, wenn Sie Windows 8.1-Apps auf universelle Windows-Apps aktualisieren und Sie sich nicht mehr auf die Unterstützung von Wischbewegungen vom Bildschirmrand auf der Plattform verlassen können.</p>
<p><strong>Neue</strong> [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927)<strong>-API:</strong>[<strong>Closing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996483), [<strong>OnClosing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996484), [<strong>Opening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996486), [<strong>OnOpening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996485), [<strong>TemplateSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn996487).</p>
<p><strong>Neue</strong> [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427)<strong>-API:</strong>[<strong>CommandBarOverflowPresenterStyle</strong>](https://msdn.microsoft.com/library/windows/apps/dn975227) und [<strong>CommandBarOverflowPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn975225).</p></td>
</tr>
<tr class="even">
<td align="left">GridView-Updates</td>
<td align="left">Vor Windows 10 war die standardmäßige Ausrichtung des Layouts [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) unter Windows horizontal und unter Windows Phone vertikal. In UWP-Apps verwendet <strong>GridView</strong> standardmäßig ein vertikales Layout für alle Gerätefamilien, um zu gewährleisten, dass Sie über eine konsistente Standardoberfläche verfügen.</td>
</tr>
<tr class="odd">
<td align="left">AreStickyGroupHeadersEnabled-Eigenschaft</td>
<td align="left">Wenn Sie gruppierte Daten in [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) oder [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) anzeigen, bleiben die Gruppenköpfe jetzt sichtbar, wenn ein Listenbildlauf durchgeführt wird. Dies ist wichtig bei großen Datensätzen, bei denen der Kopf den Kontakt für die Daten zur Verfügung stellt, die dem Benutzer angezeigt werden. In Fällen, in denen jedoch nur wenige Elemente in jeder Gruppe enthalten sind, möchten Sie vielleicht, dass die Köpfe beim Bildlauf mit den Elementen aus dem Bildschirm hinausbewegt werden. Sie können die [<strong>AreStickyGroupHeadersEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn932028)-Eigenschaft in [<strong>ItemsStackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn298795) und [<strong>ItemsWrapGrid</strong>](https://msdn.microsoft.com/library/windows/apps/dn298849) festlegen, um dieses Verhalten zu steuern.</td>
</tr>
<tr class="even">
<td align="left">GroupHeaderContainerFromItemContainer-Methode</td>
<td align="left">Wenn Sie gruppierte Daten in [<strong>ItemsControl</strong>](https://msdn.microsoft.com/library/windows/apps/br242803) anzeigen, können Sie die [<strong>GroupHeaderContainerFromItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903984)-Methode aufrufen, um einen Verweis auf den übergeordneten Kopf für die Gruppe abzurufen. Wenn beispielsweise ein Benutzer das letzte Element in einer Gruppe löscht, können Sie einen Verweis auf den Gruppenkopf abrufen und den Artikel und den Gruppenkopf zusammen entfernen.</td>
</tr>
<tr class="odd">
<td align="left">ChoosingGroupHeaderContainer-Ereignis</td>
<td align="left">Mit dem neuen [<strong>ChoosingGroupHeaderContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn899082)-Ereignis in [<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879) können Sie den Zustand für die Gruppenköpfe in [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) oder [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) festlegen. Beispielsweise möchten Sie dieses Ereignis so behandeln, dass [<strong>AutomationProperties.NameProperty</strong>](https://msdn.microsoft.com/library/windows/apps/br209099) auf den Gruppenkopf zum Darstellen der Gruppe in Hilfstechnologien festgelegt wird.</td>
</tr>
<tr class="even">
<td align="left">ChoosingItemContainer-Ereignis</td>
<td align="left">Das neue [<strong>ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989)-Ereignis in [<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879) gibt Ihnen eine bessere Kontrolle über die Virtualisierung der Benutzeroberfläche in [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) oder [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705). Verwenden Sie dieses Ereignis in Verbindung mit dem [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914)-Ereignis, um Ihre eigene Warteschlange mit wiederverwerteten Containern zu verwalten, auf die Sie bei Bedarf zugreifen. Wenn z. B. die Datenquelle durch einen Filter zurückgesetzt wurde, können Sie schnell einen bereits erstellten Satz von visuellen Elementen (ItemContainers) mit ihren Daten anpassen, um optimale Leistung zu erzielen.</td>
</tr>
<tr class="odd">
<td align="left">Virtualisierung des Listenbildlaufs</td>
<td align="left"><p>Die XAML-Steuerelemente [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) und [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) besitzen ein neues [<strong>ListViewBase.ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989)-Ereignis, das die Leistung des Steuerelements verbessert, wenn eine Änderung in der Datensammlung erfolgt.</p>
<p>Anstatt die Liste vollständig zurückzusetzen, wobei die Eingangsanimation erneut wiedergegeben wird, behält das System jetzt die derzeit in der Ansicht befindlichen Elemente sowie den Fokus und den Auswahlstatus. Neue und entfernte Artikel im Viewport werden problemlos in die Animation eingebunden bzw. ausgeblendet. Nach einer Änderung in der Datensammlung, in der Container nicht zerstört werden, kann eine App alle „alten“ Elemente schnell mit dem vorherigen Container abgleichen und die Weiterverarbeitung von Überschreibungsmethoden für den Containerlebenszyklus überspringen. Es werden nur „neue“ Elemente verarbeitet und den wiederverwendeten oder neuen Containern zugeordnet.</p></td>
</tr>
<tr class="even">
<td align="left">SelectRange-Methode und SelectedRanges-Eigenschaft</td>
<td align="left">In universellen Windows-Apps ermöglichen die Steuerelemente [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) und [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) jetzt die Auswahl von Elementen anhand von Elementindexbereichen anstelle von Elementobjektverweisen. Dies ist eine effizientere Möglichkeit, um die Auswahl von Elementen zu beschreiben, da Elementobjekte nicht für alle ausgewählten Elemente erstellt werden müssen. Weitere Informationen finden Sie unter [<strong>ListViewBase.SelectedRanges</strong>](https://msdn.microsoft.com/library/windows/apps/dn904244), [<strong>ListViewBase.SelectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904245) und [<strong>ListViewBase.DeselectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904241).</td>
</tr>
<tr class="odd">
<td align="left">Neue ListViewItemPresenter-APIs</td>
<td align="left">[<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) und [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) verwenden Elementpresenter, die die standardmäßigen visuellen Elemente für Auswahl und Fokus bereitstellen. In UWP-Apps verfügen [<strong>ListViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn298500) und [<strong>GridViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn279298) über neue Eigenschaften, mit denen Sie die visuellen Objekte für Listenelemente weiter anpassen können. Die neuen Eigenschaften sind [<strong>CheckBoxBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn913905), [<strong>CheckMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn913923), [<strong>FocusSecondaryBorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn898370), [<strong>PointerOverForeground</strong>](https://msdn.microsoft.com/library/windows/apps/dn898372), [<strong>PressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913931) und [<strong>SelectedPressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913937).</td>
</tr>
<tr class="even">
<td align="left">SemanticZoom-Updates</td>
<td align="left"><p>Das [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601)-Steuerelement hat jetzt auf allen Gerätefamilien ein einheitliches Verhalten für UWP-Apps.</p>
<p>Die Standardaktion zum Wechseln zwischen der vergrößerten Ansicht und der verkleinerten Ansicht ist, auf einen Gruppenkopf in der vergrößerten Ansicht zu tippen. Dies entspricht dem Verhalten für Windows Phone 8.1, unterscheidet sich jedoch von Windows 8.1, welches zum Zoomen die Zusammendrückbewegung verwendet. Zum Ändern der Ansichten bei Verwendung der Zusammendrückbewegung für das Zoomen legen Sie den internen [<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527) von [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601) auf [<strong>ScrollViewer.ZoomMode</strong>](https://msdn.microsoft.com/library/windows/apps/br209601)=„Enabled“ fest.</p>
<p>Für universelle Windows-Apps ersetzt die verkleinerte Ansicht die vergrößerte Ansicht und weist die gleiche Größe auf wie die ersetzte Ansicht. Dies entspricht dem Verhalten unter Windows 8.1, unterscheidet sich jedoch von Windows Phone 8.1, bei dem die verkleinerte Ansicht die volle Größe des Bildschirms belegte und über allen anderen Inhalten gerendert wurde.</p></td>
</tr>
<tr class="odd">
<td align="left">DatePicker- und TimePicker-Updates</td>
<td align="left">Das [<strong>DatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn298584)-Steuerelement und [<strong>TimePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn299280)-Steuerelement verfügen jetzt über eine konsistente Implementierung für universelle Windows-Apps auf allen Gerätefamilien. Sie weisen auch ein neues Erscheinungsbild für Windows 10 auf. Der Popupteil verwendet nun das [<strong>DatePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn625013)-Steuerelement und [<strong>TimePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn608313)-Steuerelement auf allen Geräten. Dies entspricht dem Verhalten für Windows Phone 8.1, unterscheidet sich jedoch vom Betriebssystem Windows 8.1, in dem [<strong>ComboBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209348)-Steuerelemente verwendet wurden. Mit den Flyout-Steuerelementen können Sie ganz einfach eine benutzerdefinierte Datums- und Uhrzeitauswahl erstellen.</td>
</tr>
<tr class="even">
<td align="left">Neue APIs für ScrollViewer</td>
<td align="left">[<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527) verfügt über das neue[<strong>DirectManipulationStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn858544)-Ereignis und [<strong>DirectManipulationCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn858543)-Ereignis, die die App über den Start und das Ende von Verschiebungen per Toucheingabe benachrichtigen. Sie können diese Ereignisse behandeln, um Ihre Benutzeroberfläche mit diesen Benutzeraktionen zu koordinieren.</td>
</tr>
<tr class="odd">
<td align="left">MenuFlyout-Updates</td>
<td align="left">Für universelle Windows-Apps sind neue APIs verfügbar, mit denen Sie noch einfacher bessere Kontextmenüs erstellen können. Mit der neuen [<strong>MenuFlyout.ShowAt</strong>](https://msdn.microsoft.com/library/windows/apps/dn913174)-Methode können Sie angeben, wo der Flyout in Bezug auf ein anderes Element angezeigt werden soll. (Und Ihr [<strong>MenuFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn299030) kann sogar die Grenzen des App-Fensters überlappen.) Verwenden Sie die neue [<strong>MenuFlyoutSubItem</strong>](https://msdn.microsoft.com/library/windows/apps/dn913170)-Klasse, um überlappende Menüs zu erstellen.</td>
</tr>
<tr class="even">
<td align="left">Neue Rahmeneigenschaften für ContentPresenter, Grid und StackPanel</td>
<td align="left">Allgemeine Containersteuerelemente haben neue Rahmeneigenschaften, mit denen Sie einen Rahmen um sie herum zeichnen können, ohne ein zusätzliches Rahmenelement Ihrer XAML hinzuzufügen. [<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378), [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704) und [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) verfügen über folgende neue Eigenschaften: [<strong>BorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn960179), [<strong>BorderThickness</strong>](https://msdn.microsoft.com/library/windows/apps/dn960181), [<strong>CornerRadius</strong>](https://msdn.microsoft.com/library/windows/apps/dn960183) und [<strong>Padding</strong>](https://msdn.microsoft.com/library/windows/apps/dn960187).</td>
</tr>
<tr class="odd">
<td align="left">Neue Text-APIs für ContentPresenter</td>
<td align="left">[<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378) verfügt über neue APIs, mit denen Sie die Textanzeige besser steuern können: [<strong>LineHeight</strong>](https://msdn.microsoft.com/library/windows/apps/dn903982), [<strong>LineStackingStrategy</strong>](https://msdn.microsoft.com/library/windows/apps/dn831933), [<strong>MaxLines</strong>](https://msdn.microsoft.com/library/windows/apps/dn831935) und [<strong>TextWrapping</strong>](https://msdn.microsoft.com/library/windows/apps/dn831937).</td>
</tr>
<tr class="even">
<td align="left">Visuelle Systemfokuselemente</td>
<td align="left">Visuelle Fokuselemente für XAML-Steuerelemente werden jetzt vom System erstellt und nicht mehr als XAML-Elemente in der Steuerelementvorlage deklariert. Die visuellen Fokus-Elemente sind normalerweise bei mobilen Geräten nicht erforderlich, und das Erstellen und Verwalten nach Bedarf durch das System verbessert die App-Leistung. Wenn Sie eine bessere Kontrolle über visuelle Fokuselemente benötigen, können Sie das Systemverhalten überschreiben und eine benutzerdefinierte Steuerelementvorlage zur Verfügung stellen, mit der visuelle Fokuselemente definiert werden. Weitere Informationen finden Sie unter [<strong>UseSystemFocusVisuals</strong>](https://msdn.microsoft.com/library/windows/apps/dn899076) und [<strong>IsTemplateFocusTarget</strong>](https://msdn.microsoft.com/library/windows/apps/dn899074).</td>
</tr>
<tr class="odd">
<td align="left">PasswordBox.PasswordRevealMode</td>
<td align="left"><p>In universellen Windows-Apps wird die [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867)-Eigenschaft durch die [<strong>IsPasswordRevealButtonEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/hh702579)-Eigenschaft ersetzt, um ein einheitliches Verhalten in allen Gerätefamilien zu gewährleisten.</p>
<p></p>
<div class="alert">
<strong>Achtung</strong>: Vor Windows 10 wurde die Schaltfläche für die Kennwortanzeige nicht standardmäßig angezeigt. In universellen Windows-Apps wird sie standardmäßig angezeigt. Wenn die Sicherheit Ihrer App erfordert, dass das Kennwort immer verdeckt ist, muss [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867) auf „Hidden“ festgelegt werden.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Control.IsTextScaleFactorEnabled</td>
<td align="left">Die [<strong>IsTextScaleFactorEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn624997)-Eigenschaft, die unter Windows Phone 8.1 verfügbar war, ist jetzt für universelle Windows-Apps in allen Gerätefamilien verfügbar.</td>
</tr>
<tr class="odd">
<td align="left">AutoSuggestBox</td>
<td align="left">Das [<strong>AutoSuggestBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn633874)-Steuerelement aus Windows Phone 8.1 ist jetzt für universelle Windows-Apps in allen Gerätefamilien verfügbar und sollte anstelle von [<strong>SearchBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn252771) verwendet werden. <strong>AutoSuggestBox</strong> liefert Vorschläge während der Benutzereingabe und funktioniert gut mit verschiedenen Eingabetypen wie Toucheingabe, Tastatur und Eingabemethoden-Editoren. Es verfügt außerdem über einige neue Member, damit es besser als Suchfeld funktioniert: die [<strong>QueryIcon</strong>](https://msdn.microsoft.com/library/windows/apps/dn996494)-Eigenschaft und das [<strong>QuerySubmitted</strong>](https://msdn.microsoft.com/library/windows/apps/dn996497)-Ereignis.</td>
</tr>
<tr class="even">
<td align="left">ContentDialog</td>
<td align="left">Das [<strong>ContentDialog</strong>](https://msdn.microsoft.com/library/windows/apps/dn633972)-Steuerelement aus Windows Phone 8.1 ist jetzt für universelle Windows-Apps in allen Gerätefamilien verfügbar. Mit <strong>ContentDialog</strong> können Sie ein anpassbares modales Dialogfeld anzeigen, das auf sämtlichen Geräten einwandfrei funktioniert.</td>
</tr>
<tr class="odd">
<td align="left">Pivot</td>
<td align="left">Das [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241)-Steuerelement aus Windows Phone 8.1 ist jetzt für universelle Windows-Apps in allen Gerätefamilien verfügbar. Jetzt können Sie dasselbe <strong>Pivot</strong>-Steuerelement in Ihrer App für Mobilgeräte und Desktopgeräte verwenden. <strong>Pivot</strong> bietet adaptives Verhalten, das auf Bildschirmgröße und Eingabetyp basiert. Sie können ein <strong>Pivot</strong>-Steuerelement so gestalten, dass es ein Registerkarten-ähnliches Verhalten zeigt, mit verschiedenen Ansichten der Informationen in jedem Pivot-Element.</td>
</tr>
</tbody>
</table>

 

## Text


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Windows Core-Text-APIs</td>
<td align="left"><p>Der neue [<strong>Windows.UI.Text.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958238)-Namespace verfügt über ein Client-Server-System, das die Verarbeitung von Eingaben über die Tastatur auf einem Server zentralisiert.</p>
<p>Sie können es verwenden, um den Bearbeitungspuffer Ihres benutzerdefinierten Steuerelements zur Texteingabe zu bearbeiten. Der Texteingabeserver garantiert, dass der Inhalt Ihres Steuerelements zur Texteingabe und der Inhalt seines eigenen Bearbeitungspuffers über einen asynchronen Kommunikationskanal zwischen der App und dem Server immer synchron gehalten werden.</p></td>
</tr>
<tr class="even">
<td align="left">Vektorsymbole</td>
<td align="left">Das [<strong>Glyphs</strong>](https://msdn.microsoft.com/library/windows/apps/br209921)-Element verfügt über die neue [<strong>IsColorFontEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn900468)-Eigenschaft und [<strong>ColorFontPalleteIndex</strong>](https://msdn.microsoft.com/library/windows/apps/dn900466)-Eigenschaft für die Unterstützung farbiger Schriftarten. Jetzt können Sie eine Schriftartdatei zum Rendern schriftartbasierter Symbole verwenden. Bei Verwendung von <strong>ColorFontPalleteIndex</strong> für das Wechseln der Farbpalette kann ein einzelnes Symbol mit verschiedenen Farbzusammenstellungen gerendert werden, z. B. um eine aktivierte und deaktivierte Version des Symbols anzuzeigen.</td>
</tr>
<tr class="odd">
<td align="left">Eingabemethoden-Editor für Windows-Ereignisse</td>
<td align="left">Benutzer geben manchmal Text über einen Eingabemethoden-Editor ein, der in einem Fenster direkt unterhalb eines Texteingabefelds angezeigt wird (in der Regel für ostasiatische Sprachen). Sie können das [<strong>CandidateWindowBoundsChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn955144)-Ereignis und die [<strong>DesiredCandidateWindowAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/dn955145)-Eigenschaft für [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) und [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) verwenden, damit die Benutzeroberfläche Ihrer App mit dem IME-Fenster besser funktioniert.</td>
</tr>
<tr class="even">
<td align="left">Textkompositionsereignisse</td>
<td align="left">[<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) und [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) verfügen über neue Ereignisse, die Ihre App darüber informieren, wenn Text mithilfe eines Eingabemethoden-Editors eingegeben wird: [<strong>TextCompositionStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn891458), [<strong>TextCompositionEnded</strong>](https://msdn.microsoft.com/library/windows/apps/dn891457) und [<strong>TextCompositionChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn891456). Sie können diese Ereignisse behandeln, um Ihren App-Code mit dem IME-Textkompositionsprozess zu koordinieren. Sie können z. B. eine Inlinefunktion zur automatischen Vervollständigung für ostasiatische Sprachen implementieren.</td>
</tr>
<tr class="odd">
<td align="left">Verbesserte Handhabung von bidirektionalem Text</td>
<td align="left"><p>XAML-Textsteuerelemente verfügen über eine neue API zur verbesserten Behandlung von bidirektionalem Text, was eine bessere Textausrichtung und Absatzrichtung für eine Vielzahl von Eingabesprachen ergibt.</p>
<p>Der Standardwert der [<strong>TextReadingOrder</strong>](https://msdn.microsoft.com/library/windows/apps/dn949133)-Eigenschaft wurde in <strong>DetectFromContent</strong> geändert. Dadurch ist die Unterstützung zur Erkennung der Leserichtung standardmäßig aktiviert. Die <strong>TextReadingOrder</strong>-Eigenschaft wurde auch [<strong>PasswordBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227519), [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) und [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) hinzugefügt.</p>
<p>Sie können die [<strong>TextAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/br209703)-Eigenschaft für Textsteuerelemente auf den neuen <strong>DetectFromContent</strong>-Wert festlegen, um die automatische Erkennung der Ausrichtung anhand des Inhalts zuzulassen.</p></td>
</tr>
<tr class="even">
<td align="left">Rendering von Text</td>
<td align="left">In Windows 10 wird der Text in XAML-Apps jetzt in den meisten Fällen mit der zweifachen Geschwindigkeit von Windows 8.1 gerendert. In den meisten Fällen werden Ihre Apps von diese Verbesserung ohne Änderungen profitieren. Neben dem schnelleren Rendern reduzieren diese Verbesserungen auch die typische Speicherauslastung der XAML-Apps um 5 Prozent.</td>
</tr>
</tbody>
</table>

 

## Anwendungsmodell


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Cortana</td>
<td align="left"><p>Erweitern Sie die grundlegenden Funktionen von <strong>Cortana</strong> durch Sprachbefehle, die eine einzelne Aktion in einer externen Anwendung starten und ausführen.</p>
<p><strong>Cortana</strong> integriert die grundlegenden Funktionen Ihrer App, bietet einen zentralen Einstiegspunkt, über den der Benutzer die meisten Aufgaben ohne Öffnen der App ausführen kann, und wird somit zum Bindeglied zwischen Ihrer App und dem Benutzer. In vielen Fällen spart der Benutzer dadurch viel Zeit und Mühe.</p>
<p>Hier erfahren Sie Folgendes: [integrate your app into the Cortana canvas](https://msdn.microsoft.com/library/windows/apps/xaml/dn974230) In den speziell für <strong>Cortana</strong> geschriebenen Entwurfsempfehlungen und Richtlinien für die Gestaltung der Benutzeroberfläche unter [Design basics for Universal Windows apps](https://dev.windows.com/design/design-basics) finden Sie wertvolle Anregungen.</p></td>
</tr>
<tr class="even">
<td align="left">Datei-Explorer</td>
<td align="left">Mit den neuen [<strong>Windows.System.Launcher.LaunchFolderAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn889616)-Methoden können Sie den Datei-Explorer starten und den Inhalt eines von Ihnen angegebenen Ordners anzeigen.</td>
</tr>
<tr class="odd">
<td align="left">Freigegebener Speicher</td>
<td align="left">Mit der neuen [<strong>Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager</strong>](https://msdn.microsoft.com/library/windows/apps/dn889985)-Klasse und den zugehörigen Methoden können Sie eine Datei mit einer anderen App teilen, indem Sie ein Freigabetoken übergeben, wenn Sie die andere App mithilfe der URI-Aktivierung starten. Die Ziel-App löst das Token ein, um die von der Quell-App geteilte Datei abzurufen.</td>
</tr>
<tr class="even">
<td align="left">Einstellungen</td>
<td align="left"><p>Verwenden Sie das ms-settings-Protokoll mit der [<strong>LaunchUriAsync</strong>](https://msdn.microsoft.com/library/windows/apps/hh701476)-Methode, um integrierte Einstellungsseiten anzuzeigen. Der folgende Code zeigt beispielsweise die WLAN-Einstellungsseite an.</p>
<p><code>bool result = await Launcher.LaunchUriAsync(new Uri(&quot;ms-settings://network/wifi&quot;));</code></p>
<p>Eine Liste der Einstellungsseiten, die Sie anzeigen können, finden Sie unter [How to display built-in settings pages by using the ms-settings protocol](https://msdn.microsoft.com/library/windows/apps/jj207014.aspx).</p></td>
</tr>
<tr class="odd">
<td align="left">App-zu-App-Kommunikation</td>
<td align="left"><p>Neue APIs unter Windows 10 für die [app-to-app communication](https://msdn.microsoft.com/library/windows/apps/xaml/dn997827) ermöglichen Windows-Anwendungen (und Windows-Webanwendungen) das gegenseitige Starten und das Austauschen von Daten und Dateien.</p>
<p>Dank dieser neuen APIs können komplexe Aufgaben, für die Benutzer bisher mehrere Anwendungen nutzen mussten, jetzt nahtlos durchgeführt werden. Mit Ihrer App kann beispielsweise eine App für ein soziales Netzwerk gestartet werden, um einen Kontakt auszuwählen, oder eine Kassenanwendung, um einen Bezahlvorgang durchzuführen.</p></td>
</tr>
<tr class="even">
<td align="left">App-Dienste</td>
<td align="left">Ein App-Dienst stellt für ein App eine Möglichkeit dar, für andere Apps unter Windows 10 Dienste bereitzustellen. Ein App-Dienst wird als Hintergrundaufgabe ausgeführt. Vordergrund-Apps können einen App-Dienst in einer anderen App aufrufen, um Aufgaben im Hintergrund auszuführen. Referenzinformationen zur API für App-Dienste finden Sie unter [<strong>Windows.ApplicationModel.AppService</strong>](https://msdn.microsoft.com/library/windows/apps/dn921731).</td>
</tr>
<tr class="odd">
<td align="left">App-Paketmanifest</td>
<td align="left"><p>Die Aktualisierungen der [package manifest schema](https://msdn.microsoft.com/library/windows/apps/br211474)-Referenz für Windows 10 enthalten Elemente, die hinzugefügt, entfernt oder geändert wurden.</p>
<p>Unter [Element Hierarchy](https://msdn.microsoft.com/library/windows/apps/dn934819) finden Sie Referenzinformationen zu allen Elementen, Attributen und Typen im Schema.</p></td>
</tr>
</tbody>
</table>

 

## Geräte


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Microsoft Surface Hub</td>
<td align="left"><p>Der Microsoft Surface Hub ist ein leistungsfähiges Tool für die Teamarbeit und eine großformatige Anzeigeplattform für universelle Windows-Apps, die direkt auf dem Surface Hub oder auf einem angeschlossenen Gerät laufen.</p>
<p>Entwickeln Sie eigene, speziell auf Ihr Unternehmen ausgerichtete Apps, die Vorteile wie Großbildschirm, Touch- und Freihandeingabe und umfassende integrierte Hardware wie Kameras und Sensoren optimal nutzen.</p>
<p>Unter [Design basics for Universal Windows apps](https://dev.windows.com/design/design-basics) finden Sie Entwurfsempfehlungen und Richtlinien für die Gestaltung der Benutzeroberfläche, die sich speziell auf den Surface Hub beziehen. In diesen Dokumenten werden Techniken für reaktionsfähiges Design für universelle Windows-Apps erläutert.</p>
<p>Ausführliche Informationen zur Unterstützung freigegebener Apps finden Sie unter [<strong>SharedModeSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn949019).</p>
<p>Informationen zur Freihandeingabe und Details zur Unterstützung der Multipoint-Freihandeingabe im neuen [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement finden Sie unter [<strong>Windows.UI.Input.Inking</strong>](https://msdn.microsoft.com/library/windows/apps/br208524) und [<strong>Windows.UI.Input.Inking.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958452).</p>
<p>Informationen zur Verarbeitung von Sensoreingaben finden Sie unter [Integrating devices, printers, and sensors](https://msdn.microsoft.com/library/windows/apps/br229563).</p></td>
</tr>
<tr class="even">
<td align="left">Position</td>
<td align="left"><p>In Windows 10 wird mit [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152) eine neue Methode zum Anfordern der Genehmigung des Benutzers für den Zugriff auf Positionsdaten eingeführt.</p>
<p>Der Benutzer legt den Schutz seiner Positionsdaten über <strong>Datenschutzeinstellungen für den Standort</strong> in der App <strong>Einstellungen</strong> fest. Die App kann nur unter folgenden Voraussetzungen auf die Position des Benutzers zugreifen:</p>
<ul>
<li><strong>Position dieses Geräts</strong> ist aktiviert (gilt nicht für Windows 10 für Smartphones).</li>
<li>Die Einstellung <strong>Position</strong> der Positionsdienste ist auf <strong>Ein</strong> festgelegt.</li>
<li>Für Ihre App ist unter <strong>Wählen Sie Apps aus, die Ihre Position verwenden dürfen</strong> die Einstellung <strong>Ein</strong> festgelegt.</li>
</ul>
<p>[<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152) muss aufgerufen werden, bevor Sie auf die Position des Benutzers zugreifen. Zu diesem Zeitpunkt muss sich Ihre App im Vordergrund befinden, und <strong>RequestAccessAsync</strong> muss vom UI-Thread aufgerufen werden. Solange der Benutzer Ihrer App keinen Zugriff auf seine Position gewährt hat, kann Ihre App nicht auf Positionsdaten zugreifen.</p></td>
</tr>
<tr class="odd">
<td align="left">AllJoyn</td>
<td align="left"><p>Mit dem Windows-Runtime-Namespace [<strong>Windows.Devices.AllJoyn</strong>](https://msdn.microsoft.com/library/windows/apps/dn894971) wird die Microsoft-Implementierung von AllJoyn-Open Source-Softwareframework und -diensten eingeführt. Diese APIs ermöglichen Ihrer universalen Windows-Gerät-App, sich an mit AllJoyn gesteuerten Internet der Dinge (Internet of Things, IoT)-Szenarien mit anderen Geräten zu beteiligen. Weitere Details über die AllJoyn C-APIs erhalten Sie, wenn Sie die Dokumentation unter [The AllSeen Alliance](https://allseenalliance.org/) herunterladen.</p>
<p>Mit dem in dieser Version enthaltenen Tool [AllJoynCodeGen tool](https://msdn.microsoft.com/library/windows/apps/dn913809) generieren Sie eine Windows-Komponente, mit der Sie AllJoyn-Szenarien für Ihre Geräte-App ermöglichen können.</p>
<div class="alert">
<strong>Hinweis</strong> Windows 10 IoT Core ist jetzt für eine neue Klasse kleiner Geräte verfügbar, sodass Sie IoT-Geräte (Internet der Dinge) mit Windows und Visual Studio erstellen können. Weitere Informationen zu Windows IoT finden Sie unter [WindowsOnDevices.com](http://www.windowsondevices.com/).
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Druck-APIs auf Mobilgeräten (XAML)</td>
<td align="left">Es gibt eine einzige, einheitliche Gruppe von APIs, mit denen Sie in Ihren XAML-basierten UWP-Apps auf allen Gerätefamilien, einschließlich mobiler Geräte, drucken können. Sie können Ihrer mobilen App jetzt mithilfe bekannter druckbezogener APIs aus dem [<strong>Windows.Graphics.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br226489)-Namespace und [<strong>Windows.UI.Xaml.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br243325)-Namespace eine Druckfunktion hinzufügen.</td>
</tr>
<tr class="odd">
<td align="left">Akku</td>
<td align="left"><p>Mit den Akku-APIs im [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/dn895017)-Namespace erhält Ihre App zusätzliche Informationen zu sämtlichen Akkus des Geräts, auf dem Ihre App ausgeführt wird.</p>
<p>Erstellen Sie ein [<strong>Battery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895004)-Objekt, das einen einzelnen Battery-Controller oder eine Sammlung sämtlicher Battery-Controller darstellt (wenn mit [<strong>FromIdAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn895013) oder [<strong>AggregateBattery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895011) erstellt).</p>
<p>Verwenden Sie die [<strong>GetReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895015)-Methode zum Zurückgeben eines [<strong>BatteryReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895005)-Objekts, mit dem die Ladung, die Kapazität und der Status der entsprechenden Akkus angegeben werden.</p></td>
</tr>
<tr class="even">
<td align="left">MIDI-Geräte</td>
<td align="left"><p>Mit dem neuen [<strong>Windows.Devices.Midi</strong>](https://msdn.microsoft.com/library/windows/apps/dn895002)-Namespace können Sie Folgendes erstellen:</p>
<ul>
<li>Apps, die mit externen MIDI-Geräten kommunizieren können.</li>
<li>Apps und externe Geräte, die direkt mit dem Microsoft GS MIDI-Softwaresynthesizer kommunizieren können.</li>
<li>Szenarien, in denen mehrere Clients gleichzeitig auf einen einzelnen MIDI-Port zugreifen.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Benutzerdefinierte Sensorunterstützung</td>
<td align="left">Der [<strong>Windows.Devices.Sensors.Custom</strong>](https://msdn.microsoft.com/library/windows/apps/dn895032)-Namespace ermöglicht Hardwareentwicklern die Definition neuer benutzerdefinierter Sensortypen, z. B. eines CO2-Sensors.</td>
</tr>
<tr class="even">
<td align="left">Hostbasierte Kartenemulation (HCE)</td>
<td align="left"><p>Bei der hostbasierten Kartenemulation können Sie die im Betriebssystem gehosteten NFC-Kartenemulationsdienste implementieren und per NFC-Funkverbindung trotzdem mit der externen Lesegeräteinheit kommunizieren.</p>
<p>Implementieren Sie eine Hintergrundaufgabe, um eine Smartcard per NFC zu emulieren. Verwenden Sie zum Auslösen der Hintergrundaufgabe die [<strong>SmartCardTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn624853)-Klasse.</p>
<p>Mit dem <strong>EmulatorHostApplicationActivated</strong>-Wert in der [<strong>SmartCardTriggerType</strong>](https://msdn.microsoft.com/library/windows/apps/dn608017)-Enumeration wird der App mitgeteilt, dass ein HCE-Ereignis eingetreten ist.</p></td>
</tr>
</tbody>
</table>

 

## Grafiken


|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DirectX              | DirectX 12 in Windows 10 führt die nächste Version von Microsoft Direct3D ein, der 3D-Grafik-API im Herzen von DirectX. [Direct3D 12-Grafik](https://msdn.microsoft.com/library/windows/desktop/dn903821) bietet die Effizienz und Leistung einer konsolenähnlichen API auf niedriger Ebene. Direct3D 12 ist schneller und effizienter als je zuvor. Es ermöglicht detailliertere Szenen, mehr Objekte, komplexere Effekte und eine bessere Nutzung moderner Grafikhardware.                                                                                                                                                                                                                                                                                     |
| SoftwareBitmapSource | In universellen Windows-Apps können Sie den neuen [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854)-Typ wie eine XAML-Bildquelle verwenden. Dadurch können Sie uncodierte Bilder an das XAML-Framework weiterleiten, um sie sofort auf dem Bildschirm anzuzeigen, indem Sie die Bilddecodierung durch das XAML-Framework umgehen. Sie können viel schnelleres Rendering von Bildern erreichen, z. B. das Rendern von Fotos mit niedriger Verzögerung direkt von der Kamera, mithilfe von benutzerdefinierten Bild-Decodern, Sammeln von Rahmen von DirectX-Oberflächen oder sogar im Arbeitsspeicher Bilder von Grund auf neu erstellen und alle direkt in XAML mit geringer Latenz und niedrigen Speicherbedarf rendern.                                                                                                     |
| Perspektivische Kamera   | In universellen Windows-Apps verfügt XAML über eine neue Transform3D-API, die Sie als perspektivische Transformation auf ein XAML-Struktur (oder Szene) anwenden können, was alle untergeordneten XAML-Elemente gemäß dieses einzelne Szenen-Transformation (oder Kamera) transformiert. Durch Verwendung von MatrixTransform und komplexer Mathematik war das auch früher möglich, aber Transform3D stellt eine drastische Vereinfachung dieses Effekts dar und ermöglicht auch den Effekt, animiert zu werden. Weitere Informationen finden Sie unter der [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn906919)-Eigenschaft, [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn914748), [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914714) und [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914740). |

 

## Medien


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">HTTP Live Streaming</td>
<td align="left">Sie können die neue [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912)-Klasse verwenden, um Ihren Apps anpassungsfähige Videostreamingfunktionen hinzuzufügen. Das Objekt wird initialisiert, wenn es auf eine Streaming-Manifest-Datei verweist. Zu den unterstützten Manifestformaten gehören HTTP Live Streaming (HLS) und Dynamic Adaptive Streaming over HTTP (DASH). Sobald das Objekt an ein XAML-Medienelement gebunden ist, beginnt die anpassungsfähige Wiedergabe. Eigenschaften des Datenstroms, wie etwa die verfügbare, minimale und maximale Bitrate, können abgefragt und bei Bedarf festgelegt werden.</td>
</tr>
<tr class="even">
<td align="left">Media Foundation Transcode Video Processor (XVP)-Unterstützung für Media Foundation-Transformationen (MFTs)</td>
<td align="left"><p>Windows-Apps, die Media Foundation-Transformationen (MFTs) verwenden, können jetzt den <strong>Media Foundation Transcode Video Processor</strong> (XVP) zum Konvertieren, Skalieren und Transformieren von Rohvideodaten verwenden:</p>
<p>Das neue [MF_XVP_CALLER_ALLOCATES_OUTPUT](https://msdn.microsoft.com/library/windows/desktop/dn803919)-Attribut ermöglicht die Ausgabe von Texturen, die dem Aufrufer zugeordnet sind, auch im Microsoft DirectX Video Acceleration (DXVA)-Modus.</p>
<p>Mit der neuen [<strong>IMFVideoProcessorControl2</strong>](https://msdn.microsoft.com/library/windows/desktop/dn800741)-Schnittstelle kann Ihre App Hardwareeffekte aktivieren, unterstützte Hardwareeffekte abfragen und den vom Videoprozessor durchgeführten Drehvorgang außer Kraft setzen.</p></td>
</tr>
<tr class="odd">
<td align="left">Transcodierung</td>
<td align="left">Mithilfe der neuen [<strong>MediaProcessingTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn806005)-API kann Ihre App die Medientranscodierung als Hintergrundaufgabe ausführen, sodass die Transcodierungsvorgänge auch dann fortgesetzt werden können, wenn die App im Vordergrund beendet wurde.</td>
</tr>
<tr class="even">
<td align="left">Fehlerereignisse bei MediaElement-Medien</td>
<td align="left">In universellen Windows-Apps gibt [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) auch dann Inhalte mit mehreren Streams wieder, wenn beim Decodieren eines der Streams ein Fehler auftritt (vorausgesetzt, die Medieninhalte enthalten mindestens einen gültigen Stream). Wenn z. B. bei einem Videostream in Inhalten, die einen Audio- und einen Videostream umfassen, ein Fehler auftritt, gibt <strong>MediaElement</strong> den Audiostream dennoch wieder. Das [<strong>PartialMediaFailureDetected</strong>](https://msdn.microsoft.com/library/windows/apps/dn889635)-Ereignis benachrichtigt Sie, dass einer der Streams in einem Stream nicht decodiert werden konnte. Sie werden auch informiert, bei welchem Streamtyp der Fehler aufgetreten ist, damit Sie diese Informationen in der UI wiedergeben können. Wenn in allen Streams innerhalb eines Medienstreams Fehler auftreten, wird das [<strong>MediaFailed</strong>](https://msdn.microsoft.com/library/windows/apps/br227393)-Ereignis ausgelöst.</td>
</tr>
<tr class="odd">
<td align="left">Unterstützung für adaptives Videostreaming mit MediaElement</td>
<td align="left">[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) verfügt über die neue [<strong>SetPlaybackSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn899085)-Methode, um adaptives Videostreaming zu unterstützen. Verwenden Sie diese Methode, um Ihre Medienquelle auf [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912) festzulegen.</td>
</tr>
<tr class="even">
<td align="left">Wiedergabe mit MediaElement und Image</td>
<td align="left">Das [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926)-Steuerelement und das [<strong>Image</strong>](https://msdn.microsoft.com/library/windows/apps/br242752)-Steuerelement verfügen über die neue [<strong>GetAsCastingSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn920012)-Methode. Mit dieser Methode können Sie Inhalte von allen Medien- oder Bildelementen an einen breiteren Bereich von Remotegeräten wie Miracast, Bluetooth und DLNA programmgesteuert senden.Diese Funktion ist automatisch aktiviert, wenn Sie [<strong>AreTransportControlsEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn298977) für ein <strong>MediaElement</strong> auf TRUE festlegen.</td>
</tr>
<tr class="odd">
<td align="left">Medientransport-Steuerelemente für Desktop-Apps</td>
<td align="left">Die [<strong>ISystemMediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/desktop/dn892299)-Schnittstelle und verwandte APIs ermöglichen Desktop-Apps die Interaktion mit den integrierten Systemsteuerelementen für den Medientransport. Dazu zählen die Reaktion auf Benutzerinteraktionen mit den Schaltflächen für die Transportsteuerung und die Aktualisierung der Anzeige für die Transportsteuerung, um Metadaten zu den derzeit wiedergegebenen Medieninhalten anzuzeigen.</td>
</tr>
<tr class="even">
<td align="left">JPEG-Codierung und-Decodierung mit wahlfreiem Zugriff</td>
<td align="left">Die neuen WIC-Methoden [<strong>IWICJpegFrameEncode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903864) und [<strong>IWICJpegFrameDecode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903834) ermöglichen das Codieren und Decodieren von JPEG-Bildern. Sie können jetzt auch die Indizierung der Bilddaten aktivieren, wodurch effizienter wahlfreier Zugriff auf große Bilder ermöglicht wird, der aber auch einen größeren Speicherbedarf verursacht.</td>
</tr>
<tr class="odd">
<td align="left">Überlagerungen für Medienkompositionen</td>
<td align="left">Die neue [<strong>MediaOverlay</strong>](https://msdn.microsoft.com/library/windows/apps/dn764793)-API und [<strong>MediaOverlayLayer</strong>](https://msdn.microsoft.com/library/windows/apps/dn764795)-API erleichtern das Hinzufügen von mehreren Ebenen statischer oder dynamischer Medieninhalte zu einer Medienkomposition. Deckkraft, Position und zeitliche Steuerung können für jede Ebene angepasst werden, und Sie können auch Ihren eigenen benutzerdefinierten Kompositor für Eingabeebenen implementieren.</td>
</tr>
<tr class="even">
<td align="left">Framework mit neuen Effekten</td>
<td align="left">Der [<strong>Windows.Media.Effects</strong>](https://msdn.microsoft.com/library/windows/apps/dn278802)-Namespace bietet ein einfaches und intuitives Framework zum Hinzufügen von Effekten zu Audio- und Videostreams. Das Framework enthält grundlegende Schnittstellen, die zum Erstellen von benutzerdefinierten Audio- und Videoeffekten und zum Einfügen in die Medienpipeline implementiert werden können.</td>
</tr>
</tbody>
</table>

 

## Netzwerk


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Sockets</td>
<td align="left"><p>Socketaktualisierungen:</p>
<ul>
<li><strong>Socketbroker.</strong> Der Socket-Broker kann Socket-Verbindungen im Auftrag einer App in jeder Phase des App-Lebenszyklus herstellen und beenden. So können Apps und die Dienste, die sie bereitstellen, besser gefunden werden. Zum Beispiel kann ein Win32-Dienst eingehende Socketverbindungen auch dann über den Socketbroker annehmen, wenn er nicht ausgeführt wird.</li>
<li><strong>Verbesserungen beim Durchsatz.</strong> Der Socketdurchsatz wurde für Apps optimiert, die den [<strong>Windows.Networking.Sockets</strong>](https://msdn.microsoft.com/library/windows/apps/br226960)-Namespace verwenden.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Nachbearbeitungsaufgaben für die Hintergrundübertragung</td>
<td align="left">Mit den neuen APIs im [<strong>Windows.Networking.BackgroundTransfer</strong>](https://msdn.microsoft.com/library/windows/apps/br207242)-Namespace können Sie Gruppen von Nachbearbeitungsaufgaben registrieren. Ihre App kann sofort auf erfolgreiche oder fehlerhafte Hintergrundübertragungen reagieren, auch wenn sie nicht im Vordergrund ausgeführt wird, und muss nicht warten, bis der Benutzer die App wieder startet.</td>
</tr>
<tr class="odd">
<td align="left">Bluetooth-Unterstützung für Anzeigen</td>
<td align="left">Mit dem [<strong>Windows.Devices.Bluetooth.Advertisement</strong>](https://msdn.microsoft.com/library/windows/apps/dn894325)-Namespace kann Ihre App Bluetooth LE-Werbung senden, empfangen und filtern.</td>
</tr>
<tr class="even">
<td align="left">Wi-Fi Direct-API-Update</td>
<td align="left"><p>Der Gerätebroker wird aktualisiert, um die Kopplung mit Geräten zu ermöglichen, ohne die App zu verlassen. Mithilfe der Erweiterungen des [<strong>Windows.Devices.WiFiDirect</strong>](https://msdn.microsoft.com/library/windows/apps/dn297687)-Namespace kann ein Gerät dafür sorgen, dass es für andere Geräte auffindbar ist. Außerdem kann es auf eingehende Verbindungsbenachrichtigungen lauschen.</p>
<div class="alert">
<strong>Hinweis</strong>: In dieser Version sind die Verbesserungen am Wi-Fi Direct-Feature nicht in UX integriert und unterstützen nur die Kopplung per Tastendruck. Außerdem unterstützt diese Version nur eine aktive Verbindung.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">Verbesserungen an der JSON-Unterstützung</td>
<td align="left">Der [<strong>Windows.Data.Json</strong>](https://msdn.microsoft.com/library/windows/apps/br240639)-Namespace unterstützt vorhandene Standarddefinitionen und die Entwicklerumgebung jetzt besser bei der Konvertierung von JSON-Objekten in Debugsitzungen.</td>
</tr>
</tbody>
</table>

 

## Sicherheit


|                             |                                                                      |
|-----------------------------|----------------------------------------------------------------------|
| ECC-Verschlüsselung              | Neue APIs im [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404)-Namespace bieten Unterstützung für Elliptical Curve Cryptography (ECC), eine Kryptografieimplementierung mit öffentlichem Schlüssel auf Grundlage elliptischer Kurven über begrenzten Feldern. ECC ist mathematisch komplexer als RSA, bietet kleinere Schlüsselgrößen, verringert die Arbeitsspeichernutzung und verbessert die Leistung. Sie bietet Microsoft-Diensten und -Kunden eine Alternative zu RSA-Schlüsseln und den von NIST genehmigten Kurvenparametern. |
| Microsoft Passport          | Microsoft Passport ist eine alternative Authentifizierungsmethode, bei der Kennwörter durch eine asymmetrische Verschlüsselung und eine Geste ersetzt werden. Dank der Klassen im [**Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089)-Namespace, z. B. [**KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043), ist es für Entwickler einfach, eine Anwendung mit Microsoft Passport zu erstellen, ohne die komplexen Anforderungen der Verschlüsselung oder Biometrie erfüllen zu müssen.  |
| Microsoft Passport for Work | Microsoft Passport for Work ist eine alternative Methode für die Anmeldung an Windows. Dabei wird Ihr Azure Active Directory-Konto genutzt, für das keine Kennwörter, Smartcards und virtuellen Smartcards verwendet werden. Sie können auswählen, ob Sie diese Richtlinieneinstellung aktivieren oder deaktivieren möchten. |
| Token Broker                | Token Broker ist ein neues Authentifizierungs-Framework, mit dem es Apps erleichtert wird, eine Verbindung mit Online-Identitätsanbietern (z. B. Facebook) herzustellen. Die Features, z. B. die Verwaltung von Kontobenutzernamen und -kennwörtern und eine optimierte UI, sorgen für eine deutlich verbesserte Authentifizierung für die Benutzer. |

 

## Systemdienste


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Stromversorgung</td>
<td align="left"><p>Ihre Windows-Desktopanwendung kann jetzt darüber benachrichtigt werden, ob der Stromsparmodus aktiviert oder deaktiviert ist. Durch die Berücksichtigung der veränderten Leistungsaufnahme kann die Anwendung dazu beitragen, die Akkulaufzeit zu verlängern.</p>
<p>[<strong>GUID_POWER_SAVING_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/hh448380): Verwenden Sie diese neue GUID mit der [<strong>PowerSettingRegisterNotification</strong>](https://msdn.microsoft.com/library/windows/desktop/hh769082)-Funktion, um darüber benachrichtigt zu werden, wann der Stromsparmodus aktiviert oder deaktiviert ist.</p>
<p>[<strong>SYSTEM_POWER_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/aa373232): Diese Struktur wurde aktualisiert, um den Stromsparmodus zu unterstützen. Die vierte Option, <strong>SystemStatusFlag</strong> (früher „Reserved1“), gibt jetzt an, ob der Stromsparmodus aktiviert ist oder nicht. Verwenden Sie die [<strong>GetSystemPowerStatus</strong>](https://msdn.microsoft.com/library/windows/desktop/aa372693)-Funktion, um einen Zeiger auf diese Struktur abzurufen.</p></td>
</tr>
<tr class="even">
<td align="left">Version</td>
<td align="left"><p>Mithilfe der [Version Helper functions](https://msdn.microsoft.com/library/windows/desktop/dn424972) können Sie die Version des Betriebssystems ermitteln. Unter Windows 10 wurden diese Hilfsfunktionen um die neue Funktion [<strong>IsWindows10OrGreater</strong>](https://msdn.microsoft.com/library/windows/desktop/dn905474) erweitert. Zur Abfrage der Systemversion sollten Sie diese Hilfsfunktionen anstelle der veralteten [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451)-Funktion und [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439)-Funktion verwenden. Weitere Informationen zum Abrufen der Systemversion finden Sie unter [ Getting the System Version](https://msdn.microsoft.com/library/windows/desktop/ms724429).</p>
<p>Wenn Sie die veraltete [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451)-Funktion oder [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439)-Funktion verwenden, um die Versionsinformationen in einer [<strong>OSVERSIONINFOEX</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724833)-Struktur oder [<strong>OSVERSIONINFO</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724834)-Struktur abzurufen, müssen Sie berücksichtigen, dass die darin enthaltenen Versionsnummern von 6.3 für Windows 8.1 und Windows Server 2012 R2 auf 10.0 für Windows 10 erhöht werden. Weitere Informationen zu Versionsnummern des Betriebssystems finden Sie unter [Operating System Version](https://msdn.microsoft.com/library/windows/desktop/ms724832).</p>
<p>Außerdem müssen Sie Ihre Anwendung explizit auf Windows 8.1 oder Windows 10 ausrichten, um mithilfe der [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451)-Funktion oder [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439)-Funktion die richtigen Versionsinformationen für diese Versionen abzurufen. Weitere Informationen dazu, wie Sie die Anwendung auf diese Windows-Versionen ausrichten, finden Sie unter [Targeting your application for Windows](https://msdn.microsoft.com/library/windows/desktop/dn481241).</p></td>
</tr>
<tr class="odd">
<td align="left">Benutzerinformationen</td>
<td align="left">Neue APIs im [<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814)-Namespace erleichtern Ihnen den Zugriff auf Informationen zum Benutzer, z. B. auf den Benutzernamen und das Profilbild. Außerdem ist es möglich, auf Benutzerereignisse zu reagieren, z. B. das Anmelden und Abmelden.</td>
</tr>
<tr class="even">
<td align="left">Speicherverwaltung und Profilerstellung</td>
<td align="left">Die Unterstützung der API zur Erstellung von Arbeitsspeicherprofilen in [<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) wurde auf alle Plattformen ausgedehnt, und die Gesamtfunktionalität wurde durch neue Klassen und Funktionen erweitert.</td>
</tr>
</tbody>
</table>

 

## Speicherung


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Für Windows Phone verfügbare APIs für die Dateisuche</td>
<td align="left"><p>Als App-Herausgeber können Sie dem App-Manifest Erweiterungen hinzufügen und so Ihre App registrieren, damit sie mit anderen von Ihnen veröffentlichten Apps einen gemeinsamen Speicherordner nutzt. Rufen Sie dann mithilfe der [<strong>Windows.Storage.ApplicationData.GetPublisherCacheFolder</strong>](https://msdn.microsoft.com/library/windows/apps/dn889607)-Methode den freigegebenen Speicherort auf.</p>
<p>Das starke Sicherheitskonzept von Windows-Runtime-Apps verhindert gewöhnlich, dass Apps Daten untereinander teilen. Für Apps des gleichen Herausgebers kann es aber nützlich sein, Dateien und Einstellungen auf Benutzerbasis freizugeben.</p></td>
</tr>
</tbody>
</table>

 

## Tools


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Visuelle Live-Struktur in Visual Studio</td>
<td align="left">Visual Studio verfügt über das neue Feature „Visuelle Live-Struktur“. Sie können es während des Debuggens verwenden, um den Zustand der visuellen Struktur Ihrer App schnell zu erkennen und zu entdecken, wie Eigenschaften festgelegt wurden. Außerdem können Sie Eigenschaftswerte ändern, während die App ausgeführt wird. Somit können Sie experimentieren und optimieren, ohne erneut starten zu müssen.</td>
</tr>
<tr class="even">
<td align="left">Ablaufprotokollierung</td>
<td align="left"><p>[TraceLogging](https://msdn.microsoft.com/library/windows/desktop/dn904636) ist eine neue Ereignisablauf-API für Benutzermodus-Apps und Kernelmodustreiber. Sie basiert auf [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803) (ETW). Diese API bietet eine vereinfachte Möglichkeit, Code zu instrumentieren und strukturierte Daten mit Ereignissen einzubeziehen, ohne dass eine separate Manifest-XML-Datei für die Instrumentierung erforderlich ist.</p>
<p>TraceLogging-APIs für WinRT, .NET und C/C++ sind für verschiedene Entwicklerzielgruppen verfügbar.</p></td>
</tr>
</tbody>
</table>

 

## Benutzerfreundlichkeit


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Spracherkennung</td>
<td align="left">Die universelle Windows-Plattform unterstützt jetzt die kontinuierliche Spracherkennung für Diktierszenarien mit langer Aufnahmezeit. In den Dokumenten zur Sprachinteraktion erfahren Sie, wie das kontinuierliche Diktieren ermöglicht wird.</td>
</tr>
<tr class="even">
<td align="left">Drag & Drop-Funktionen zwischen verschiedenen Anwendungsplattformen</td>
<td align="left"><p>Die neuen [<strong>Windows.ApplicationModel.DataTransfer.DragDrop</strong>](https://msdn.microsoft.com/library/windows/apps/dn894216)-Namespaces stellen universellen Windows-Apps Drag & Drop-Funktionen zur Verfügung. Früher waren mit universellen Windows-Apps keine gängigen Drag & Drop-Szenarien für Desktopprogramme möglich, wie etwa das Ziehen eines Dokuments aus einem Ordner in eine Outlook-E-Mail, um es anzufügen. Mit diesen neuen APIs kann Ihre App Benutzern das einfache Verschieben von Daten zwischen verschiedenen universellen Windows-Apps und dem Desktop ermöglichen.</p>
<p>Zur Unterstützung von Drag & Drop zwischen Apps wurden diese neuen APIs zu XAML hinzugefügt:</p>
<ul>
<li>[<strong>ListViewBase.DragItemsCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn831959)</li>
<li>[<strong>UIElement</strong>](https://msdn.microsoft.com/library/windows/apps/br208911): [<strong>CanDrag</strong>](https://msdn.microsoft.com/library/windows/apps/dn903558), [<strong>DragStarting</strong>](https://msdn.microsoft.com/library/windows/apps/dn903560), [<strong>StartDragAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn903562), [<strong>DropCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn903561)</li>
<li>[<strong>DragOperationDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831917), [<strong>DragUI</strong>](https://msdn.microsoft.com/library/windows/apps/dn985714), [<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985715)</li>
<li>[<strong>DragEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/br242372): [<strong>AcceptedOperation</strong>](https://msdn.microsoft.com/library/windows/apps/dn831912), [<strong>DataView</strong>](https://msdn.microsoft.com/library/windows/apps/dn831913), [<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985710), [<strong>GetDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831914), [<strong>Modifiers</strong>](https://msdn.microsoft.com/library/windows/apps/dn831915)</li>
<li>[<strong>DragItemsCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn831953), [<strong>DropCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903549), [<strong>DragStartingEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903540)</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Benutzerdefinierte Titelleisten</td>
<td align="left">Für UWP-Apps für die Desktop-Gerätefamilie können Sie nun die [<strong>ApplicationViewTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906115)-Klasse mit der [<strong>ApplicationView.TitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906131)-Eigenschaft und [<strong>Window.SetTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn965560)-Methode verwenden, um den Inhalt der standardmäßigen Windows-Titelleiste durch Ihren eigenen benutzerdefinierten XAML-Inhalt zu ersetzen. Die XAML wird als „system chrome“, behandelt, damit Windows die Eingabeereignisse anstelle Ihrer App behandelt. Dies bedeutet, dass der Benutzer weiterhin ziehen und die Größe des Fensters verändern kann, auch dann, wenn er auf den Inhalt Ihrer benutzerdefinierten Titelleiste klickt.</td>
</tr>
</tbody>
</table>

 

## Web


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Internet Explorer</td>
<td align="left"><p>Internet Explorer führt den Edgemodus ein: ein neuer „Live“-Dokumentmodus, der für maximale Interoperabilität mit anderen modernen Browsern und Webinhalten entwickelt wurde. Dieser experimentelle Modus wird nach und nach einer zufällig ausgewählten Gruppe von Windows 10-Benutzern bereitgestellt. Sie können den Edgemodus über den neuen IE-Mechanismus <strong>about:flags</strong> manuell aktivieren oder deaktivieren. Weitere Informationen:</p>
<ul>
<li>[Living on the Edge – our next step in helping the web just work](http://blogs.msdn.com/b/ie/archive/2014/11/11/living-on-the-edge-our-next-step-in-interoperability.aspx).</li>
<li>[The Internet Explorer for Windows 10 Developer Guide](https://dev.windows.com/microsoft-edge/).</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Browsen im WebView-Edgemodus</td>
<td align="left">Das [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702)-Steuerelement verwendet das gleiche Renderingmodul wie der neue Edge-Browser. Dadurch wird der genaueste standardkonforme HTML-Renderingmodus zur Verfügung gestellt.</td>
</tr>
<tr class="odd">
<td align="left">Hintergrundthread-WebView</td>
<td align="left">Sie können [<strong>WebView.ExecutionMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn932034) angeben, um die Verarbeitung und Anzeige von Webinhalten in einem separaten Hintergrundthread zu ermöglichen. Dies kann die Leistung bei bestimmten spezifischen Szenarien verbessern.</td>
</tr>
<tr class="even">
<td align="left">WebView.UnsupportedUriSchemeIdentified-Ereignis</td>
<td align="left"><p>Mit dem neuen [<strong>WebView.UnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn974400)-Ereignis können Sie entscheiden, wie Ihre App mit einem nicht unterstützten URI-Schema verfahren soll. Sie können dieses Ereignis behandeln, damit Ihre App eine benutzerdefinierte Behandlung von nicht unterstützten URI-Schemas bereitstellen kann.</p>
<p>Informationen zum HTML-WebView-Steuerelement finden Sie unter dem [<strong>MSWebViewUnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn803906.aspx)-Ereignis.</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.NewWindowRequested-Ereignis</td>
<td align="left"><p>Mit dem neuen [<strong>WebView.NewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974397)-Ereignis können Sie reagieren, wenn ein Skript in WebView ein neues Browserfenster anfordert.</p>
<p>Informationen zum HTML-WebView-Steuerelement finden Sie unter dem [<strong>MSWebViewNewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030)-Ereignis.</p></td>
</tr>
<tr class="even">
<td align="left">WebView.PermissionRequested-Ereignis</td>
<td align="left"><p>Mit dem neuen [<strong>WebView.PermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974398)-Ereignis können für WebView-Inhalte die umfassenden neuen HTML5-APIs genutzt werden, die eine spezielle Erlaubnis durch den Benutzer erfordern, z. B. bei Geolocation.</p>
<p>Informationen zum HTML-WebView-Steuerelement finden Sie unter dem [<strong>MSWebViewPermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030.aspx)-Ereignis.</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.UnviewableContentIdentified-Ereignis</td>
<td align="left"><p>Das neue [<strong>WebView.UnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn299351)-Ereignis ermöglicht es Ihnen, zu reagieren, wenn in [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) zu Nicht-Webinhalten navigiert wird, z. B. zu einer PDF-Datei oder einem Office-Dokument.</p>
<p>Informationen zu HTML-WebView-Steuerelementen finden Sie unter dem [<strong>MSWebViewUnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn609716)-Ereignis.</p></td>
</tr>
<tr class="even">
<td align="left">WebView.AddWebAllowedObject-Methode</td>
<td align="left"><p>Sie können die neue [<strong>WebView.AddWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn903993)-Methode aufrufen, um ein WinRT-Objekt in eine XAML-[<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) einzufügen, um dann ihre Funktionen über vertrauenswürdiges JavaScript aufzurufen, das in dieser <strong>WebView</strong> gehostet wird. Beispielsweise Systembenachrichtigungen in Webinhalt angezeigt werden, indem eine Anforderung gestellt wird, dass die übergeordnete App die [<strong>ToastNotificationManager</strong>](https://msdn.microsoft.com/library/windows/apps/br208642)-WinRT-API aufruft.</p>
<p>Informationen zum HTML-[<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702)-Steuerelement finden Sie unter der [<strong>addWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn926632)-Methode.</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.ClearTemporaryWebDataAync-Methode</td>
<td align="left">Wenn ein Benutzer mit Webinhalten innerhalb einer XAML-[<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) interagiert, führt das <strong>WebView</strong>-Steuerelement basierend auf der Sitzung dieses Benutzers eine Zwischenspeicherung der Daten durch. Rufen Sie die neue [<strong>ClearTemporaryWebDataAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn974394)-Methode auf, um diesen Cache zu löschen. Beispielsweise können Sie den Cache löschen, wenn sich ein Benutzer von der App abmeldet, damit ein anderer Benutzer nicht auf die Daten aus der vorherigen Sitzung zugreifen kann.</td>
</tr>
</tbody>
</table>



<!--HONumber=Mar16_HO5-->


