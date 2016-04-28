---
description: Navigation in UWP-Apps (Universelle Windows-Plattform) basiert auf flexiblem Modell aus Navigationsstrukturen, Navigationselementen, Funktionen auf Systemebene
title: Navigationsdesigngrundlagen für UWP-Apps (Univ. Windows-Plattform)
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigationsdesigngrundlagen
template: detail.hbs
---

#  Navigationsdesigngrundlagen für UWP-Apps


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Die Navigation in UWP-Apps (Universelle Windows-Plattform) basiert auf einem flexiblen Modell aus Navigationsstrukturen, Navigationselementen und Funktionen auf Systemebene. Gemeinsam ermöglichen sie eine Reihe intuitiver Benutzererfahrungen für die Navigation zwischen Apps, Seiten und Inhalten.

In einigen Fällen kann es möglich sein, alle Inhalte und Funktionen der App auf einer einzelnen Seite anzuordnen, ohne dass der Benutzer mehr tun muss, als durch Verschieben in den Inhalten zu navigieren. Die meisten Apps verfügen jedoch in der Regel über mehrere Seiten mit Inhalten und Funktionen, die der Benutzer aufrufen und mit denen er interagieren kann. Wenn eine App mehrere Seiten hat, müssen Sie die geeignete Navigationsfunktionalität bereitstellen.

Damit sie fehlerlos und intuitiv von den Benutzern verwendet werden kann, umfasst die mehrseitige Navigation in UWP-Apps Folgendes (später ausführlich beschrieben):

-   **Die richtige Navigationsstruktur**

    Das Erstellen einer für Benutzer sinnvollen Navigationsstruktur ist für die Entwicklung einer intuitiven Navigationsfunktionalität entscheidend.

-   **Kompatible Navigationselemente**, die die ausgewählte Struktur unterstützen.

    Navigationselemente können dem Benutzer helfen, zum gewünschten Inhalt zu gelangen, und sie können auch anzeigen, wo sich der Benutzer in der App befindet. Allerdings belegen sie auch Platz, der für Inhalte oder Steuerungselemente genutzt werden könnte. Daher ist es wichtig, dass Sie die für Ihre App-Struktur geeigneten Navigationselemente verwenden.

-   **Geeignete Reaktionen auf Navigationsfeatures auf Systemebene (z. B. „Zurück”)**

    Um eine einheitliche intuitive Benutzererfahrung zu bieten, reagieren Sie in vorhersehbarer Weise auf Navigationsfeatures auf Systemebene.

## <span id="Build_the_right_navigation_structure"></span><span id="build_the_right_navigation_structure"></span><span id="BUILD_THE_RIGHT_NAVIGATION_STRUCTURE"></span>Erstellen der richtigen Navigationsstruktur


Betrachten Sie eine App als eine Sammlung von Seitengruppen, wobei jede Seite einen einzigartigen Satz von Inhalten oder Funktionen enthält. So besitzt eine Foto-App beispielsweise eine Seite für die Aufnahme von Fotos, eine Seite für die Bildbearbeitung und eine andere Seite für die Verwaltung Ihrer Bildbibliothek. Die Anordnung dieser Seiten in Gruppen definiert die Navigationsstruktur der App. Es gibt zwei allgemeine Methoden zum Anordnen einer Gruppe von Seiten:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">In einer Hierarchie</th>
<th align="left">Als Peers</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><img src="images/nav/nav-pages-hiearchy.png" alt="Pages arranged in a hierarchy" /></p></td>
<td align="left"><p><img src="images/nav/nav-pages-peer.png" alt="Pages arranged as peers" /></p></td>
</tr>
<tr class="even">
<td align="left"><p>Seiten sind in einer Baumstruktur organisiert. Jede untergeordnete Seite hat nur ein übergeordnetes Element. Ein übergeordnetes Element kann jedoch eine oder mehrere untergeordnete Seiten haben. Um eine untergeordnete Seite aufzurufen, durchlaufen Sie das übergeordnete Element.</p></td>
<td align="left"><p>Die Seiten existieren nebeneinander. Sie können in beliebiger Reihenfolge von einer Seite zur nächsten wechseln.</p></td>
</tr>
</tbody>
</table>

 

Eine typische App verwendet beide Anordnungen, wobei einige Teile als Peers und einige Teile in Hierarchien angeordnet sind.

![App mit einer Hybridstruktur](images/nav/nav-hybridstructure.png.png)

Wann sollten Sie also Seiten in Hierarchien und wann als Peers anordnen? Zur Beantwortung dieser Frage müssen Sie die Anzahl der Seiten in der Gruppe berücksichtigen. Sie müssen feststellen, ob die Seiten in einer bestimmten Reihenfolge durchlaufen werden, und Sie müssen die Beziehung zwischen den Seiten ermitteln. Im Allgemeinen sind flachere Strukturen einfacher zu verstehen und schneller zu navigieren, aber manchmal ist es angemessen, eine tiefe Hierarchie zu verwenden.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Wir empfehlen eine hierarchische Beziehung in folgenden Fällen:</p>
<ul>
<li><p>Sie erwarten, dass der Benutzer die Seiten in einer bestimmten Reihenfolge durchlaufen wird. Sie ordnen die Hierarchie entsprechend an, um die Reihenfolge zu erzwingen.</p></li>
<li><p>Es gibt eine klare Beziehung bestehend aus einem übergeordneten und untergeordneten Elementen zwischen einer der Seite und den anderen Seiten in der Gruppe.</p></li>
<li><p>Bei mehr als 7 Seiten in der Gruppe.</p>
<p>Wenn eine Gruppe mehr als 7 Seiten umfasst, fällt es den Benutzern möglicherweise schwer, zu verstehen, wie einzigartig die Seiten sind, oder ihre aktuelle Position innerhalb der Gruppe zu ermitteln. Wenn Sie davon ausgehen, dass dies kein Problem für Ihre App ist, machen Sie aus den Seiten Peers. Ziehen Sie andernfalls eine hierarchische Struktur in Betracht, um die Seiten in zwei oder mehr kleinere Gruppen zu unterteilen. (Ein Hub-Steuerelement kann Ihnen dabei helfen, die Seiten in Kategorien zu gruppieren.)</p></li>
</ul></td>
<td align="left"><p>Wir empfehlen eine hierarchische Peer-Beziehung in folgenden Fällen:</p>
<ul>
<li>Die Seiten können in beliebiger Reihenfolge angezeigt werden.</li>
<li>Die Seiten sind deutlich voneinander abgegrenzt und verfügen nicht über eine offensichtliche Beziehung zwischen über- und untergeordneten Elementen.</li>
<li><p>Es gibt weniger als acht Seiten in der Gruppe.</p>
<p>Wenn eine Gruppe mehr als 7 Seiten umfasst, fällt es den Benutzern möglicherweise schwer, zu verstehen, wie einzigartig die Seiten sind, oder ihre aktuelle Position innerhalb der Gruppe zu ermitteln. Wenn Sie davon ausgehen, dass dies kein Problem für Ihre App ist, machen Sie aus den Seiten Peers. Ziehen Sie andernfalls eine hierarchische Struktur in Betracht, um die Seiten in zwei oder mehr kleinere Gruppen zu unterteilen. (Ein Hub-Steuerelement kann Ihnen dabei helfen, die Seiten in Kategorien zu gruppieren.)</p></li>
</ul></td>
</tr>
</tbody>
</table>

 

## <span id="Use_the_right_navigation_elements"></span><span id="use_the_right_navigation_elements"></span><span id="USE_THE_RIGHT_NAVIGATION_ELEMENTS"></span>Verwenden der richtigen Navigationselementen


Navigationselemente können zwei Aufgaben erfüllen: Dadurch kann der Benutzer den gewünschten Inhalt abrufen, und einige Elemente zeigen Benutzern auch an, wo sie sich in der App befinden. Allerdings benötigen diese auch Platz, den die App für Inhalte oder Steuerungselemente nutzen könnte, daher ist es wichtig, dass Sie die für Ihre App-Struktur geeigneten Navigationselemente verwenden.

### <span id="Peer-to-peer_navigation_elements"></span><span id="peer-to-peer_navigation_elements"></span><span id="PEER-TO-PEER_NAVIGATION_ELEMENTS"></span>Peer-to-Peer-Navigationselemente

Peer-zu-Peer-Navigationselemente ermöglichen die Navigation zwischen Seiten auf der gleichen Ebene in der gleichen Unterstruktur.

![Peer-to-Peer-Navigation](images/nav/nav-lateralmovement.png)

Bei der Peer-to-Peer-Navigation empfehlen wir die Nutzung von Registerkarten oder eines Navigationsbereichs.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Navigationselement</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Tabs and pivot](../controls-and-patterns/tabs-pivot.md)</p>
<p><img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></p></td>
<td align="left">Zeigt eine dauerhafte Liste mit Links zu Seiten auf derselben Ebene an.
<p>Verwenden Sie in folgenden Fällen Registerkarten/Pivots:</p>
<ul>
<li><p>Es sind 2 bis 5 Seiten vorhanden.</p>
<p>(Sie können Registerkarten/Pivots bei mehr als fünf Seiten verwenden, es kann aber schwierig sein, alle Registerkarten/Pivots auf dem Bildschirm anzuzeigen.)</p></li>
<li>Sie erwarten, dass Benutzer häufig zwischen Seiten wechseln werden.</li>
</ul>
<p>Dieser Entwurf für eine App, die Restaurants findet, verwendet Registerkarten/Pivots:</p>
<p><img src="images/food-truck-finder/uap-foodtruck-tabletphone-sbs-sm-400.png" alt="Example of an app using tabs/pivots pattern" /></p></td>
</tr>
<tr class="even">
<td align="left"><p>[Nav pane](../controls-and-patterns/nav-pane.md)</p>
<p><img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></p></td>
<td align="left">Zeigt eine Liste mit Links zu den übergeordneten Seiten an.
<p>Verwenden Sie in folgenden Fällen einen Navigationsbereich:</p>
<ul>
<li>Sie erwarten nicht, dass Benutzer häufig zwischen Seiten wechseln werden.</li>
<li>Sie möchten Platz sparen, wobei Sie eine Verlangsamung der Navigationsvorgänge in Kauf nehmen.</li>
<li>Die Seiten befinden sich auf der obersten Ebene.</li>
</ul>
<p>Dieser Entwurf einer Smart Home-App enthält einen Navigationsbereich:</p>
<p><img src="images/smart-home/uap-smarthome-tabletphone-sbs-sm-400.png" alt="Example of an app that uses a nav pane pattern" /></p>
<p></p></td>
</tr>
</tbody>
</table>

 

Wenn die Navigationsstruktur über mehrere Ebenen verfügt, empfehlen wir, dass Peer-to-Peer-Navigationselemente nur mit den Peers innerhalb der aktuellen Unterstruktur verknüpft sind. Beachten Sie die folgende Abbildung, die eine Navigationsstruktur mit drei Ebenen zeigt:

![App mit zwei Unterstrukturen](images/nav/nav-subtrees.png)
-   Auf Ebene 1 sollte das Peer-to-Peer-Navigationselement Zugriff auf die Seiten A, B, C und D ermöglichen.
-   Auf Ebene 2 sollten die Peer-to-Peer Navigationselemente für die A2-Seiten nur mit den anderen A2-Seiten verknüpft werden. Sie sollten nicht mit Seiten auf Ebene 2 in der C-Unterstruktur verknüpft sein.

![App mit zwei Unterstrukturen](images/nav/nav-subtrees2.png)

### <span id="Hierarchical_navigation_elements"></span><span id="hierarchical_navigation_elements"></span><span id="HIERARCHICAL_NAVIGATION_ELEMENTS"></span>Hierarchische Navigationselemente

Hierarchische Navigationselemente ermöglichen die Navigation zwischen einer übergeordneten Seite und den untergeordneten Seiten.

![Hierarchische Navigation](images/nav/nav-verticalmovement.png)

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Navigationselement</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Hub](../controls-and-patterns/hub.md)</p>
<p><img src="images/higsecone-hub-thumb.png" alt="Hub" /></p></td>
<td align="left">Ein Hub ist eine spezielle Art von Navigationssteuerelement, das eine Vorschau/Übersicht der untergeordneten Seiten bereitstellt. Im Gegensatz zum Navigationsbereich oder den Registerkarten ermöglicht es die Navigation zu diesen untergeordneten Seiten über in die Seite selbst eingebettete Links und Abschnittsüberschriften.
<p>Verwenden Sie in folgenden Fällen einen Hub:</p>
<ul>
<li>Sie gehen davon aus, dass Benutzer einen Teil des Inhalts der untergeordneten Seiten anzeigen möchten, ohne zu jeder einzeln navigieren zu müssen.</li>
</ul>
<p>Hubs fördern das Erkennen und Erforschen, weshalb sie ideal für Medien, Newsreader und Shopping-Apps geeignet sind.</p>
<p></p></td>
</tr>
<tr class="even">
<td align="left"><p>[Master/details](../controls-and-patterns/master-details.md)</p>
<p><img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></p></td>
<td align="left">Zeigt eine Liste (Masteransicht) der Elementübersichten an. Durch Auswahl eines Elements wird die entsprechende Elementseite im Detailbereich angezeigt.
<p>Verwenden Sie in folgenden Fällen das Master/Details-Element:</p>
<ul>
<li>Sie erwarten, dass Benutzer häufig zwischen untergeordneten Elementen wechseln werden.</li>
<li>Sie möchten es dem Benutzer ermöglichen, Vorgänge auf hoher Ebene, z. B. Löschen oder Sortieren, für einzelne Elemente oder Gruppen von Elementen durchzuführen, und Sie möchten es dem Benutzer ermöglichen, Details für jedes Element anzuzeigen oder zu aktualisieren.</li>
</ul>
<p>Master/Details-Elemente eignen sich gut für E-Mail-Posteingänge, Kontaktlisten und die Dateneingabe.</p>
<p>Dieser Entwurf für eine Aktien-App verwendet ein Master/Details-Muster:</p>
<p><img src="images/stock-tracker/uap-finance-tabletphone-sbs-sm.png" alt="Example of a stock trading app that has a master/details pattern" /></p></td>
</tr>
</tbody>
</table>

 

### <span id="Historical_navigation_elements"></span><span id="historical_navigation_elements"></span><span id="HISTORICAL_NAVIGATION_ELEMENTS"></span>Historische Navigationselemente

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Navigationselement</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Zurück</td>
<td align="left"><p>Der Benutzer soll den Navigationsverlauf innerhalb einer App und, abhängig vom Gerät, auch von App zu App durchlaufen können. Weitere Informationen finden Sie im Abschnitt [Make your app work well with system-level navigation features](#backnavigation) , der weiter unten in diesem Artikel aufgeführt ist.</p></td>
</tr>
</tbody>
</table>

 

### <span id="Content-embedded_navigation_elements"></span><span id="content-embedded_navigation_elements"></span><span id="CONTENT-EMBEDDED_NAVIGATION_ELEMENTS"></span>Im Inhalt eingebettete Navigationselemente

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Navigationselement</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Hyperlinks und Schaltflächen</td>
<td align="left"><p>Im Inhalt eingebettete Navigationselemente werden im Inhalt einer Seite angezeigt. Im Gegensatz zu anderen Navigationselementen, die für alle Gruppen oder Unterstrukturen der Seite konsistent sein sollten, sind im Inhalt eingebettete Navigationselemente auf jeder Seite einzigartig.</p></td>
</tr>
</tbody>
</table>

 

### <span id="Combining_navigation_elements"></span><span id="combining_navigation_elements"></span><span id="COMBINING_NAVIGATION_ELEMENTS"></span>Kombinieren von Navigationselementen

Sie können Navigationselemente kombinieren, um eine für Ihre App geeignete Navigationserfahrung zu erstellen. Ihre App kann beispielsweise einen Navigationsbereich verwenden, um auf Seiten auf oberster Ebene und Registerkarten auf Seiten der zweiten Ebene zuzugreifen.


\[Dieser Artikel enthält spezielle Informationen zu UWP-Apps und Windows 10. Laden Sie für Windows 8.1 die [PDF-Datei mit Windows 8.1-Richtlinien](https://go.microsoft.com/fwlink/p/?linkid=258743) herunter.\]



 






<!--HONumber=Mar16_HO1-->


