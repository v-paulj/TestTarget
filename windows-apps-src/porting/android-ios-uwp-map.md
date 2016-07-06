---
author: GrantMeStrength
Description: Vergleichen Sie Plattformfunktionen zwischen iOS, Android und Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: "Windows-Apps-Konzeptzuordnung für Android- und iOS-Entwickler"
ms.sourcegitcommit: de5420b45832a482d08e5e7ede436407f7dbf2af
ms.openlocfilehash: 074a71bf3d037004ca376c11b58d17c906f804a5

---

#Windows-Apps-Konzeptzuordnung für Android- und iOS-Entwickler

Wenn Sie Entwickler mit Android- oder iOS-Kenntnissen sind und/oder über den entsprechenden Code verfügen und zu Windows 10 und zur universellen Windows-Plattform (UWP) wechseln möchten, bietet Ihnen dieser Artikel alle Informationen, die Sie für die Zuordnung der Plattformfeatures – und Ihrer Kenntnisse – zwischen den drei Plattformen benötigen.

Siehe auch die Portierungsinformationen in [Wechsel von iOS zu UWP](ios-to-uwp-root.md). Dieses Dokument kann [heruntergeladen](https://www.microsoft.com/en-us/download/details.aspx?id=52041) werden.

## Benutzeroberfläche


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Entwurfssprache.</strong><br><br>Eine Reihe von Konventionen für das Aussehen und Verhalten von Apps auf der Plattform.</td>
<td align="left"><strong>Die Richtlinien von Material Design für Android</strong> bieten eine visuelle Sprache für Android-Designer und -Entwickler.</td>
<td align="left"><strong>Die Richtlinien für Eingabegeräte</strong> bieten Empfehlungen für iOS-Designer und -Entwickler.</td>
<td align="left"><a href="https://dev.windows.com/design"><strong>Der Artikel zum Entwerfen einer UWP-App-Benutzeroberfläche für Windows</strong></a> zeigt Ihnen, wie Sie eine App erstellen, die auf allen Windows 10-Geräten fantastisch aussieht. Dort finden Sie Grundlagen zum Benutzeroberflächenentwurf, Techniken für reaktionsfähiges Design und eine umfassende Liste ausführlicher Richtlinien.<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Markupsprache für die Benutzeroberfläche.</strong> <br><br>Eine Markupsprache, mit der die Benutzeroberfläche und ihre Komponenten gerendert und beschrieben werden. Jede Plattform bietet einen Editor für die visuelle Bearbeitung und Markupbearbeitung.<br/></td>
<td align="left"><strong>XML-Layouts</strong>, mit <strong>Android Studio</strong> oder <strong>Eclipse</strong> bearbeitet.</td>
<td align="left"><strong>XIB</strong> und <strong>Storyboards</strong> mit <strong>Interface Builder</strong> in Xcode bearbeitet.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185595.aspx">XAML</a></strong>, mit <strong><a href="https://www.visualstudio.com/">Microsoft Visual Studio</a></strong> und <strong><a href="https://msdn.microsoft.com/library/jj171012.aspx">Blend für Visual Studio</a></strong> bearbeitet.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228259.aspx">XAML-Plattform</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228349.aspx">Erstellen einer Benutzeroberfläche mit XAML</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228350.aspx">Definieren von Layouts mit XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Integrierte Steuerelemente der Benutzeroberfläche.</strong> <br><br>Von der Plattform bereitgestellte wiederverwendbare UI-Elemente, z. B. Schaltflächen, Listensteuerelemente und Textsteuerelemente.</td>
<td align="left">Vordefinierte <strong>Ansichtsklassen</strong> und <strong>Ansichtsgruppenklassen</strong>, die als Widgets, Layouts, Textfelder, Container, Datums- und Uhrzeitsteuerelemente und Expertensteuerelemente bezeichnet werden.</td>
<td align="left"><strong>Ansichten</strong> und <strong>Steuerelemente</strong> in der Xcode-Objektbibliothek und im UIKit-Benutzeroberflächenkatalog. Ansichten umfassen Bildansichten, Auswahlansichten und Bildlaufansichten. Steuerelemente umfassen Schaltflächen, Datumsauswahl und Textfelder.</td>
<td align="left">Die XAML-Plattform bietet Ihnen einen reichhaltigen Satz <strong>integrierter Steuerelemente</strong>, z. B. Schaltflächen, Listensteuerelemente, Panels, Textsteuerelemente, Befehlsleisten, Auswahl, Medien und Freihandeingabe.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Steuern der Ereignisbehandlung.</strong> <br><br>Definieren der Logik, die ausgeführt wird, wenn in UI-Steuerelementen Ereignisse ausgelöst werden.</td>
<td align="left"><strong>Ereignishandler</strong> und <strong>Ereignislistener</strong> werden in XML oder programmgesteuert hinzugefügt.</td>
<td align="left">Steuerelemente senden <strong>Action</strong>-Meldungen an <strong>Ziele</strong>.</td>
<td align="left">Sie können Methoden zum Behandeln der Ereignisse eines XAML-Steuerelements in einer <strong>CodeBehind-Datei</strong>, die der XAML-Seite zugeordnet ist, definieren. <strong>Ereignishandler</strong> werden immer in Code geschrieben. Sie können jedoch diese Handler im XAML-Markup oder im Code mit Ereignissen verbinden.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185584.aspx">Übersicht über Ereignisse und Routingereignisse</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Datenbindung.</strong> <br><br>Ein Softwaredesignmuster, das der App-UI das Rendern von Daten und optional das Synchronisieren mit diesen Daten ermöglicht.</td>
<td align="left">Es wird eine <strong>Datenbindungsbibliothek</strong> bereitgestellt, bislang jedoch nur als Betaversion.</td>
<td align="left">In iOS ist kein integriertes Bindungssystem vorhanden. <strong>Key-Value Observing</strong> kann als Grundlage für die Datenbindung verwendet werden, entweder mithilfe einer Drittanbieterbibliothek oder durch das Schreiben von zusätzlichem Code. Steuerelemente verwenden zum Abrufen von Daten Delegate/Rückrufe.</td>
<td align="left">Die <strong>Datenbindung</strong> erfolgt durch die UWP-Plattform. Mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt204783.aspx">{x:Bind}</a></strong>-Markuperweiterung nutzen Sie eine Bindung oder <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt204782.aspx">{Binding}</a></strong> mit hoher Leistung, um mehr Funktionen zur Verfügung zu haben. Sie können die Bindung so konfigurieren, dass auf der Benutzeroberfläche mithilfe von <strong>unidirektionaler Bindung</strong> Werte aus einer Datenquelle angezeigt werden oder dass mithilfe von <strong>bidirektionaler Bindung</strong> diese Werte außerdem beobachtet werden und die Benutzeroberfläche aktualisiert wird, wenn sie sich ändern.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt210947.aspx">Datenbindung</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Benutzeroberflächenautomatisierung.</strong> <br><br>Der programmgesteuerte Zugriff auf UI-Elemente ermöglicht den Zugriff auf Apps durch Hilfstechnologieprodukte und die Interaktion von automatisierten Testskripts mit der Benutzeroberfläche.</td>
<td align="left"><strong>Beschriftungen</strong>, <strong>contentDescription</strong> und <strong>hint</strong>-Werte erleichtern die automatisierte Suche von UI-Elementen. Android Studio ermöglicht Ihnen mit den Testumgebungen <strong>UI Automator</strong> und <strong>Espresso</strong> das Schreiben von UI-Tests.</td>
<td align="left">Mit dem <strong>Automation-Instrument</strong> können Sie automatisierte UI-Testskripts schreiben, die mithilfe der <strong>Accessibility</strong>-Einstellungen Elemente identifizieren oder die die Position des Elements in der <strong>Elementhierarchie</strong> ermitteln.</td>
<td align="left">Über die <strong><a href="https://msdn.microsoft.com/library/windows/apps/ee684076.aspx">Benutzeroberflächenautomatisierung</a></strong> erhalten Sie standardmäßig programmgesteuerten Zugriff auf integrierte UI-Elemente in UWP.<br/><strong><a href="https://msdn.microsoft.com/library/windows/apps/mt297667.aspx">Benutzerdefinierte Automatisierungspeers</a></strong> ermöglichen Ihnen die Bereitstellung von Automatisierungsunterstützung für eigene benutzerdefinierte UI-Klassen. Das <strong><a href="https://msdn.microsoft.com/library/dd286726.aspx#VerifyingCodeUsingCUITCreate">Projekt mit Tests der codierten UI</a></strong> in Visual Studio ermöglicht Ihnen das Testen Ihrer gesamten Anwendung über die UI oder das Testen der isolierten UI.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Ändern der Darstellung eines Steuerelements.</strong> <br><br>Bearbeiten von Größe, Farbe und anderen Attributen.</td>
<td align="left">Steuerelemente verfügen über <strong>Eigenschaften</strong>, die mithilfe des Entwicklungstools, im XML-Markup oder programmgesteuert bearbeitet werden können.</td>
<td align="left">Steuerelemente verfügen über <strong>Attribute</strong>, die Sie mit dem <strong>Attributes Inspector</strong> in Interface Builder oder programmgesteuert bearbeiten können.</td>
<td align="left">Sie können mit Visual Studio und Blend für Visual Studio die <strong>Eigenschaften</strong> von Steuerelementen im XAML-Markup oder programmgesteuert bearbeiten.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt228345.aspx">Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Wiederverwendbare visuelle Stile.</strong> <br><br>Anwenden visueller Änderungen in einem wiederverwendbaren Format auf eine Reihe von Steuerelementen.</td>
<td align="left"><strong>XML Styles</strong> sind Gruppen von Eigenschaften, die auf ein oder mehrere Steuerelemente angewendet werden.</td>
<td align="left">In iOS wird die Wiederverwendung visueller Stile nicht standardmäßig unterstützt, jedoch ermöglicht das UIAppearance-Protokoll die gemeinsame Nutzung allgemeiner Attribute durch mehrere Steuerelemente.</td>
<td align="left">Sie können wiederverwendbare <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx">Styles</a></strong> erstellen, die auf mehrere Steuerelemente angewendet und für die einfache Wiederverwendung in einem <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.resourcedictionary.aspx">ResourceDictionary</a></strong> gespeichert werden können.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465381.aspx">Schnellstart: Entwerfen von Steuerelementen</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Bearbeiten der visuellen Struktur von Steuerelementen.</strong> <br><br>Sie können die visuelle Struktur eines Steuerelements umfassender anpassen, als lediglich Eigenschaften oder Attribute zu ändern, z. B. den Kontrollkästchentext unter das Kontrollkästchen verschieben.</td>
<td align="left">In Android gibt es keine einfache Methode zum Bearbeiten der visuellen Struktur von Steuerelementen.</td>
<td align="left">In iOS gibt es keine einfache Methode zum Bearbeiten der visuellen Struktur von Steuerelementen.</td>
<td align="left">Um die visuelle Struktur eines Steuerelements anzupassen, können Sie seine <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.controltemplate.aspx">Steuerelementvorlage</a></strong> im XAML-Markup kopieren und bearbeiten.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465374.aspx">Schnellstart: Steuerelementvorlagen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Integrierte Touchgesten.</strong> <br><br>Bereitstellen von angepasster Touchunterstützung durch die Behandlung allgemeiner Gestenereignisse, z. B. Tippen und Doppeltippen in Ansichten und Steuerelementen.</td>
<td align="left"><strong>GestureDetectors</strong> erkennen allgemeine Touchgesten, einschließlich Bildlauf, langes Drücken, Tippen, Doppeltippen und schnelles Wischen.</td>
<td align="left">Das UIKit-Framework bietet integrierte <strong>Gestenerkennungen</strong>, die Touchgesten, einschließlich Tippen, Zusammendrücken, Schwenken, Wischen, Drehen und langes Drücken, erkennen.</td>
<td align="left"><strong>UI-Elemente</strong> ermöglichen Ihnen, <strong>statische Gestenereignisse</strong> wie Tippen, Doppeltippen, Rechtstippen und Halten sowie <strong>Manipulationsgestenereignisse</strong> wie Ziehen, Wischen, Drehen, Zusammendrücken und Aufziehen zu behandeln. Gestenereignisse sind <strong>Routingereignisse</strong> und können von übergeordneten Objekten, die das untergeordnete UIElement enthalten, behandelt werden.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185617.aspx">Toucheingabe-Interaktionen</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185599.aspx#gestures__manipulations__and_interactions">Benutzerdefinierte Benutzerinteraktionen – Gesten, Manipulationen und Interaktionen</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">Navigations- und App-Struktur</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Layouts.</strong> <br><br>Das Layout definiert die Struktur der Benutzeroberfläche.</td>
<td align="left">Das Layout besteht aus <strong>Ansichtsgruppen</strong>, z. B. <strong>LinearLayout</strong> und <strong>RelativeLayout</strong>, in denen andere Ansichtsgruppen oder Ansichten geschachtelt werden können.</td>
<td align="left">Das Layout besteht aus einem <strong>UIViewController</strong>, der <strong>UIViews</strong> enthält, die geschachtelt werden können.</td>
<td align="left">XAML bietet ein flexibles Layoutsystem, das aus <strong>Layoutpanel-Klassen</strong> wie <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.canvas.aspx">Canvas</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.aspx">Grid</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.relativepanel.aspx">RelativePanel</a></strong> und <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx">StackPanel</a></strong> für statische und dynamische Layouts besteht. <strong><a href="https://msdn.microsoft.com/library/ms171352.aspx">Eigenschaften</a></strong> dienen zum Steuern der Größe und Position der Elemente.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx">Definieren von Layouts mit XAML</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Peer-to-Peer-Navigation.</strong> <br><br>Bereitstellen von Methoden für den Benutzer, um zwischen Seiten auf der gleichen Hierarchieebene zu navigieren.</td>
<td align="left"><strong>Registerkarten</strong>, <strong>Wischbewegungen</strong> und <strong>Navigation Drawers</strong> stellen eine <strong>laterale Navigation</strong> bereit.</td>
<td align="left"><strong>TabBarController</strong>, <strong>SplitViewController</strong> und <strong>PageViewController</strong> ermöglichen die Navigation zwischen Ansichten auf gleicher Hierarchieebene.</td>
<td align="left">Sie können eine dauerhafte Liste von Links/Registerkarten über dem Inhalt mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997788.aspx">Registerkarten/Pivots</a></strong> anzeigen. Mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997787.aspx">Navigationsbereich/geteilter Ansicht</a></strong> können Sie neben dem Inhalt eine Liste von Links anzeigen.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187344.aspx">Navigation</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt465735.aspx">Peer-zu-Peer-Navigation zwischen zwei Seiten</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Hierarchische Navigation.</strong> <br><br>Navigieren zwischen über- und untergeordneten Seiten einer Hierarchie.</td>
<td align="left"><strong>Listen</strong> und <strong>Rasterlisten</strong>, <strong>Schaltflächen</strong> und weitere Steuerelemente bieten eine <strong>absteigende Navigation</strong>, wenn Sie mit <strong>Intents</strong> zum Laden weiterer <strong>Aktivitäten</strong> verwendet werden.</td>
<td align="left"><strong>Navigation Controller</strong> ermöglichen Benutzern die Navigation zwischen den Ebenen einer Hierarchie.</td>
<td align="left"><strong><a href="https://msdn.microsoft.com/library/windows/apps/dn449149.aspx">Hubs</a></strong> zeigen dem Benutzer eine Vorschau von Inhalten an, die ausgewählt werden können, um zwischen untergeordneten Seiten zu navigieren. <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn997765.aspx">Master/Details</a></strong> ermöglicht dem Benutzer aus einer Liste von Elementübersichten auszuwählen, die neben dem entsprechenden Detailabschnitt angezeigt werden.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187344.aspx">Navigation</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Navigation per Schaltfläche „Zurück“.</strong> <br><br>Rückwärtsnavigation in einer Anwendung.</td>
<td align="left">Die <strong>Zurück</strong>- und <strong>Nach-oben</strong>-Schaltfläche auf der Aktionsleiste bieten <strong>aufsteigende</strong> und <strong>temporale</strong> Navigation mithilfe des <strong>Zurück-Stapels</strong>.</td>
<td align="left">Dem <strong>Navigation Controller</strong> kann eine Zurück-Schaltfläche hinzugefügt werden.<br/></td>
<td align="left">Mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.backstack.aspx">BackStack-Eigenschaft</a></strong>, die dem Benutzer das Durchlaufen des <strong>Navigationsverlaufs</strong> ermöglicht, lässt sich das Betätigen der Zurück-Schaltfläche bzw. Zurück-Taste einfach handhaben.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt465734.aspx">Navigation per Schaltfläche „Zurück“</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Begrüßungsbildschirm.</strong> <br><br>Das Anzeigen eines Bilds beim Starten der App, hauptsächlich für Branding verwendet.</td>
<td align="left">Begrüßungsbildschirme werden nicht standardmäßig bereitgestellt und werden durch Bearbeiten des ersten <strong>Designhintergrunds</strong> für Aktivitäten implementiert.</td>
<td align="left">Apps müssen über ein <strong>statisches Startbild</strong> oder eine <strong>XIB-/Storyboard-Startdatei</strong> verfügen.</td>
<td align="left">Sie erstellen einen Begrüßungsbildschirm mithilfe eines <strong>Bilds</strong> und eines farbigen Hintergrunds. <a href="https://msdn.microsoft.com/library/windows/apps/mt187309.aspx">Die Anzeige eines Begrüßungsbildschirms kann verlängert werden</a>.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187306.aspx">Hinzufügen eines Begrüßungsbildschirms</a></td>
</tr>
</tbody>
</table>
<h2 id="custom-inputs">Benutzerdefinierte Eingaben</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Spracheingabe.</strong> <br><br>Spracherkennung für die Spracheingabe und zusätzliche Sprachfunktionen.</td>
<td align="left">Die Spracheingabe kann durch jede App bereitgestellt werden, die einen <strong>RecognizerIntent</strong> implementiert, z. B. die <strong>Google-Sprachsuche</strong>. Die <strong>SpeechRecognizer</strong>-Klasse ermöglicht Apps die Verwendung der Spracherkennungs-API von Google.</td>
<td align="left">Es ist keine integrierte Spracherkennungs- oder Spracheingabe-API vorhanden.</td>
<td align="left">Sie können für die Interaktion mit der App im Vordergrund die <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185615.aspx">Spracherkennungs</a></strong>-API verwenden. Mithilfe von sprachbasierten <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185598.aspx">Cortana-Interaktionen</a></strong> können Sie Apps im Vordergrund oder Hintergrund starten und mit Hintergrund-Apps interagieren.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185614.aspx">Sprachinteraktionen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Benutzerdefinierte Benutzereingaben.</strong> <br><br>Behandeln von Eingaben per Tastatur, Maus, Stift und anderen Eingaben.</td>
<td align="left">Die Unterstützung von Interaktionen umfasst <strong>Berührung</strong>, <strong>Touchpad</strong>, <strong>Eingabestift</strong>, <strong>Maus</strong> und <strong>Tastatur</strong>. Bewegungen und Eingaben werden auf die gleiche Weise wie Berührung gemeldet, es lassen sich jedoch weitere Informationen über das <strong>Eingabegerät</strong> ermitteln.</td>
<td align="left">Es wird Unterstützung für <strong>Berührung</strong>, <strong>Apple Pencil</strong> und <strong>Hardwaretastaturen</strong> bereitgestellt.</td>
<td align="left">Sie finden Unterstützung für eine Vielzahl von Interaktionen, einschließlich <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185617.aspx">Berührung</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187313.aspx">Touchpad</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187311.aspx">Zeichen- bzw. Eingabestift</a></strong> mit Freihandschrift, <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt187308.aspx">Maus</a></strong> und <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt185607.aspx">Tastatur</a></strong>. Ihre Apps können die Daten behandeln, ohne Informationen über das verwendete Eingabegerät zu benötigen, und bei Bedarf kann auf Daten von Rohdateneingabegeräten zugegriffen werden.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt404610.aspx">Behandeln von Zeigereingaben</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185599.aspx">Benutzerdefinierte Benutzerinteraktionen</a></td>
</tr>
</tbody>
</table>
<h2 id="data">Daten</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Lokale App-Daten.</strong> <br><br>Lokales Speichern von Einstellungen und Dateien für die App.</td>
<td align="left">Lokale Dateien können mit <strong>openFileOutput</strong> und <strong>openFileInput</strong> gespeichert werden. Auf die Einstellungen in einer <strong>SharedPreferences-Datei</strong> kann mithilfe von <strong>getSharedPreferences</strong> zugegriffen werden.</td>
<td align="left">Lokale Dateien können im Verzeichnis <strong>Application Support</strong> gespeichert werden, auf das über die <strong>NSFileManager</strong>-Klasse zugegriffen wird. Der Zugriff auf die Einstellungen in <strong>Preference</strong>-Dateien erfolgt über die <strong>NSUserDefaults</strong>-Klasse.</td>
<td align="left">Mit den <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/br230562.aspx">Windows.Storage</a></strong>-Klassen werden lokale Daten auf einheitliche Weise gespeichert. Einstellungen werden als <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdatacontainer.aspx">ApplicationDataContainer</a></strong>-Objekt gespeichert, auf das mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdata.localsettings.aspx">ApplicationData.LocalSettings</a></strong>-Eigenschaft zugegriffen kann. Dateien werden in einem <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefolder.aspx">StorageFolder</a></strong>-Objekt gespeichert, auf das mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.storage.applicationdata.localfolder.aspx">ApplicationData.LocalFolder</a></strong>-Eigenschaft zugegriffen werden kann.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt299098.aspx">Speichern und Abrufen von Einstellungen und anderen App-Daten</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Lokaler Datenbankspeicher.</strong> <br><br>Speichern von App-Daten in einer relationalen Datenbank, ggf. mit objektrelationalem Mapping (ORM).</td>
<td align="left">Die <strong>SQLite</strong>-Datenbank wird bereitgestellt. ORM ist nicht integriert. SQL-Abfragen werden mit der <strong>SQLiteDatabase</strong>-Klasse ausgeführt.</td>
<td align="left">Die <strong>SQLite</strong>-Datenbank wird bereitgestellt. <strong>CoreData</strong>, das integrierte Objektdiagrammframework, kann mit SQLite verwendet werden und mit ORM vergleichbare Funktionalität bereitstellen.</td>
<td align="left">Sie können Daten mithilfe <strong>SQLite</strong> speichern. <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592863.aspx">Entity Framework</a></strong> ist ein integriertes ORM, mit dem die Notwendigkeit entfällt, umfangreichen Datenzugriffscode zu schreiben, und mit dem Sie die Datenbank einfach abfragen können, ohne SQL zu schreiben. Sie können SQL-Abfragen direkt mit der <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592864.aspx">SQLite-Bibliothek</a> ausführen.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592862.aspx">Datenzugriff</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>HTTP-Bibliotheken für den Zugriff auf REST-APIs.</strong> <br><br>Integrierte Bibliotheken, die Ihnen die Kommunikation mit Webdiensten und Webservern per HTTP(S) ermöglichen.<br/></td>
<td align="left">HTTP-Bibliotheken <strong>HttpURLConnection</strong> und <strong>Volley</strong>.</td>
<td align="left"><strong>NSURLSession</strong>, <strong>NSURLConnection</strong> und <strong>NSURLDownload</strong>.</td>
<td align="left">Die integrierte <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpclient">HttpClient</a></strong>-API bietet Ihnen Zugriff auf allgemeine HTTP-Funktionen, z. B. GET, DELETE, PUT, POST, allgemeine Authentifizierungsmuster, SSL, Cookies und Statusinformationen.</td>
</tr>
<tr class="even">
<td align="left"><strong>Cloudsicherungsdienste.</strong> <br><br>Von der Plattform bereitgestellte Sicherungsdienste für App-Daten.</td>
<td align="left">Mit dem <strong>Backup Manager</strong> von Android werden Anwendungsdaten im <strong>Android Sicherungsdienst</strong> von Google gesichert.</td>
<td align="left"><strong>Das iCloud-Backup</strong> kann vom Benutzer zum Durchführen der Sicherungen, einschließlich der App-Daten, konfiguriert werden. Apps, die mit iCloud kompatible <strong>Core Data</strong>, den <strong>iCloud-Schlüssel-Wert-Speicher</strong> und die <strong>Dokumentenspeicherung in iCloud</strong> verwenden.</td>
<td align="left">Alle App-Daten, die Sie mithilfe der APIs für das Roaming von <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.aspx">ApplicationData</a></strong> (einschließlich <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.roamingfolder.aspx">RoamingFolder</a></strong> und <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.roamingsettings.aspx"><strong>RoamingSettings</strong></a>) speichern, werden automatisch mit der Cloud und anschließend auch mit den anderen Geräten des Benutzers synchronisiert. Die Synchronisierung erfolgt über das Microsoft-Konto des Benutzers.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/hh465094.aspx">Richtlinien für das Roaming von App-Daten</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>HTTP-Dateidownloads.</strong> <br><br>Herunterladen von kleinen und großen Dateien per HTTP.</td>
<td align="left"><strong>URLConnection</strong> und <strong>HTTPURLConnection</strong> werden für den Download per HTTP und FTP verwendet. Für den Download im Hintergrund kann auch der <strong>Download-Manager</strong> des Systems verwendet werden.</td>
<td align="left"><strong>Für den Download von Dateien per HTTP und FTP können NSURLSession</strong> und <strong>NSURLConnection</strong> verwendet werden.</td>
<td align="left">Mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.backgroundtransfer.aspx">API für Hintergrundübertragungen</a></strong> können Sie Dateien zuverlässig per HTTP(S) und FTP übertragen und dabei das Anhalten der App sowie Verbindungsunterbrechungen berücksichtigen und Anpassungen entsprechend Konnektivität und Akkulaufzeit vornehmen. Sie können auch die <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.httpclient.aspx">HttpClient</a></strong>-Klasse verwenden, die sich besonders für kleinere Dateien eignet.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">Welche Netzwerktechnologie?</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280377.aspx">Hintergrundübertragungen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Sockets.</strong> <br><br>Erstellen von UDP-Datagrammsockets und TCP-Sockets als Low-Level-Schnittstelle für die Kommunikation mit anderen Geräten unter Verwendung des eigenen Protokolls.</td>
<td align="left"><strong>Die Socket</strong>-Klasse stellt TCP-Sockets bereit, und die <strong>DatagramSocket</strong>-Klasse stellt einen UDP-Socket bereit.</td>
<td align="left"><strong>NSStream</strong> und <strong>CFStream</strong> stellen TCP-Sockets bereit, und <strong>CFSocket</strong> stellt UDP-Sockets bereit.</td>
<td align="left">Sie können die <strong><a href="https://msdn.microsoft.com/library/windows/apps/br241319">DatagramSocket</a></strong>-Klasse für die Kommunikation mit einem UDP-Datagrammsocket und die <strong><a href="https://msdn.microsoft.com/library/windows/apps/br226882">StreamSocket</a></strong>-Klasse für die Kommunikation per TCP oder Bluetooth RFCOMM verwenden.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280233.aspx">Netzwerkgrundlagen</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">Welche Netzwerktechnologie?</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280234.aspx">Übersicht über Sockets</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>WebSockets.</strong> <br><br>Bieten bidirektionale Kommunikation zwischen einem Client und einem Server und ermöglichen die Datenübertragung in Echtzeit.</td>
<td align="left">In Android sind keine WebSockets-Bibliotheken integriert.</td>
<td align="left">In iOS sind keine WebSockets-Bibliotheken integriert.</td>
<td align="left">Sichere Verbindungen mit Servern, die WebSockets unterstützen, können mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.messagewebsocket.aspx">MessageWebSocket</a></strong>-Klasse hergestellt werden, um kleinere Nachrichten mit Empfangsbestätigung zu senden, und mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.networking.sockets.streamwebsocket.aspx">StreamWebSocket</a></strong>, um größere Binärdateien zu übertragen, die in Abschnitten gelesen werden können.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280233.aspx">Netzwerkgrundlagen</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt280235.aspx">Welche Netzwerktechnologie?</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt186447.aspx">Übersicht über WebSockets</a></td>
</tr>
<tr class="even">
<td align="left"><strong>OAuth-Bibliotheken.</strong> <br><br>OAuth-Bibliotheken ermöglichen den Zugriff auf OAuth-Drittanbieter und jede in die Plattform integrierte Kontoverwaltung.</td>
<td align="left">Es wird keine generische OAuth-Bibliothek bereitgestellt. Für die OAuth-Authentifizierung bei Google Play-Diensten wird die <strong>GoogleAuthUtil</strong>-Klasse bereitgestellt.<br/></td>
<td align="left">Es wird keine generische OAuth-Bibliothek bereitgestellt. Das <strong>Accounts Framework</strong> bietet Zugriff auf Benutzerkonten, die bereits auf dem Gerät gespeichert sind, z. B. Facebook und Twitter.</td>
<td align="left">Mit dem <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270196.aspx">Webauthentifizierungsbroker</a></strong> aus der generischen OAuth-Bibliothek können Sie eine Verbindung mit Identitätsanbieterdiensten von Drittanbietern herstellen. Die Benutzer können im <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270189.aspx">Schließfach für Anmeldeinformationen</a></strong> ihre Anmeldeinformationen speichern und sie auf mehreren Geräten verwenden. Der <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn896755.aspx">Microsoft.Live</a></strong>-Namespace ermöglicht Ihnen den einfachen Zugriff auf das Live SDK-OAuth-Protokoll für den Zugriff auf Microsoft-Dienste.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt270184.aspx">Authentifizierung und Benutzeridentität</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.aspx">Windows.Security.Authentication.Web-API-Dokumentation</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">WebAuthenticationBroker-Codebeispiel</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">Tools</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>IDE.</strong> <br><br>Das zum Erstellen der App verwendete Toolset.</td>
<td align="left"><strong>Android Studio</strong> und <strong>Eclipse</strong>, wobei Google Entwicklern die Verwendung von Android Studio empfiehlt.</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://www.visualstudio.com/features/universal-windows-platform-vs.aspx">Visual Studio</a></strong> und <strong><a href="https://msdn.microsoft.com/library/jj171012.aspx">Blend für Visual Studio</a></strong> verfügen über alle Tools, die Sie zum Codieren, Entwerfen, Verbinden, Debuggen, Analysieren, Optimieren und Testen von UWP-Apps benötigen. Visual Studio bietet Ihnen außerdem <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt188754.aspx">Emulatoren</a></strong> für Windows 10-Geräte, sodass Sie Ihre App auf unterschiedlichen emulierten Geräten testen können.<br/><br/><a href="https://dev.windows.com/downloads">Downloads und Tools für UWP</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Codeorganisation.</strong> <br><br>Die grundlegende Ordnerstruktur einer App, häufig anhand einer anfänglichen Vorlage erstellt.</td>
<td align="left"><strong>Die Android Manifest</strong>-Datei, <strong>java</strong>-Ordner mit Quelldateien, <strong>res</strong>-Ordner mit Ressourcen, einschließlich Layouts und Werten, <strong>Gradle</strong>-Buildskripts in Android Studio und <strong>Ant</strong>-Buildskripts in Eclipse.</td>
<td align="left">Quelldateien und <strong>Supporting Files</strong>, Datei <strong>Info.plist</strong>, <strong>Main.storyboard</strong> und <strong>LaunchScreen.storyboard</strong>. In <strong>Objektbibliotheken</strong> gespeicherte Images.</td>
<td align="left">Die UWP-App enthält XAML- und Codedateien für die App mit den Namen „Example.xaml“ und „Example.xaml.cs“, verschiedene Images im Ordner <strong>Assets</strong>, eine Startseite, z. B. <strong>MainPage.xaml</strong> oder <strong>MainPage.xaml.cs</strong>, und ein Manifest.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn765018.aspx">Erstellen der App „Hello, world“ (XAML)</a></td>
</tr>
</tbody>
</table>
<h2 id="app-lifecycle">App-Lebenszyklus</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>App-Lebenszyklus.</strong> <br><br>Behandeln von Ereignissen beim Starten, Anhalten, Fortsetzen und Schließen der App, mit der Möglichkeit, den Anwendungszustand zu speichern/wiederherzustellen und weitere Aufgaben auszuführen.</td>
<td align="left">Jede Aktivität verfügt über einen eigenen <strong>Aktivitätslebenszyklus</strong> mit Zuständen, z. B. <strong>resumed</strong>. <strong>Lebenszyklusrückrufe</strong> wie <strong>onResume</strong> werden in den <strong>Aktivitätsklassen</strong> implementiert.</td>
<td align="left">Der <strong>Anwendungslebenszyklus</strong> weist Zustände, z. B. <strong>suspended</strong>, auf. Methoden, z. B. <strong>applicationDidEnterBackground:</strong>, werden im <strong>appDelegate</strong>-Objekt implementiert, um bei Zustandsänderungen Code auszuführen.</td>
<td align="left">Die Anwendung weist die <strong>App-Ausführungszustände</strong> „NotRunning“, „Activated“, „Runningׅ“, „Suspending“, „Suspended“ und „Resuming“ auf.<br/><br/>Sie können die Methoden „OnLaunched“, „OnActivated“, „Suspending“ oder „Resuming“ der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.aspx">Application-Klasse</a></strong> implementieren, um Code auszuführen, wenn sich der Zustand ändert.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt243287.aspx">App-Lebenszyklus</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Hintergrundaufgaben.</strong> <br><br>Aufgaben, die Vorgänge im Hintergrund ausführen und weiterhin ausgeführt werden, wenn die App nicht mehr im Vordergrund ausgeführt wird.</td>
<td align="left">Apps können <strong>Dienste</strong> starten, die Hintergrundvorgänge ausführen, wenn die App nicht mehr im Vordergrund ausgeführt wird. Dienste besitzen einen eigenen <strong>Lebenszyklus</strong> und werden im Manifest registriert.</td>
<td align="left"><strong>Die Ausführung im Hintergrund</strong> ist nur für bestimmte Aufgabentypen zulässig.<br/><br/>Apps deklarieren <strong>unterstützte Hintergrundaufgaben</strong> in der Datei „Info.plist“ mithilfe der <strong>UIBackgroundModes</strong>.<br/><br/>Das System steuert, wann und wie lange Hintergrundaufgaben ausgeführt werden.</td>
<td align="left">Sie können eine Hintergrundaufgabe erstellen, indem Sie die <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.aspx">IBackgroundTask</a></strong>-Schnittstelle implementieren und die Aufgabe im Anwendungsmanifest registrieren. Sie können festlegen, dass eine Aufgabe durch einen <a href="https://msdn.microsoft.com/library/windows/apps/mt186458.aspx"><strong>Timer</strong></a>, <a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtriggertype.aspx"><strong>Systemtrigger</strong></a> oder <a href="https://msdn.microsoft.com/library/windows/apps/mt185632.aspx"><strong>Wartungsauslöser</strong></a> ausgelöst wird.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299103.aspx">Unterstützen Ihrer App mit Hintergrundaufgaben</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299100.aspx">Erstellen und Registrieren einer Hintergrundaufgabe</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187310.aspx">Richtlinien für Hintergrundaufgaben</a></td>
</tr>
</tbody>
</table>
<h2 id="performance">Leistung</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Bewährte Methoden zur Leistungssteigerung.</strong> <br><br>Richtlinien für das Erstellen von schnellen, dynamischen und akkuschonenden Apps mit kurzer Startzeit.</td>
<td align="left">Android bietet das Schulungshandbuch <strong>Best Practices for Performance</strong> (Bewährte Methoden zur Leistungssteigerung).</td>
<td align="left">iOS bietet das Dokument <strong>Performance Overview</strong> (Übersicht über die Leistung).</td>
<td align="left">Sie können das ausführliche <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt270266.aspx">Handbuch zur Leistung</a></strong> lesen, das unter anderem Abschnitte zu folgenden Themen enthält: Festlegen von Leistungszielen, Messen der Leistung, Speicherverwaltung, flüssige Animationen, effizienter Dateisystemzugriff und verfügbare Tools für Profilerstellung und Leistung.</td>
</tr>
<tr class="even">
<td align="left"><strong>Ansichtsoptimierung für eine reaktionsfähige Benutzeroberfläche.</strong> <br><br>Verbessern der Leistung durch Optimieren der Ansichten.</td>
<td align="left">Das Optimieren der <strong>Layouthierarchien</strong> mit dem Tool Hierarchy Viewer, das <strong>Wiederverwenden von Layouts</strong> und das Laden von <strong>Ansichten bei Bedarf</strong> sind Verfahren, mit denen Sie die Reaktionsfähigkeit des UI-Threads erhalten und verhindern, dass &quot;ANR&quot;-Meldungen (<strong>Application Not Responding</strong>) ausgegeben werden.<br/></td>
<td align="left">Beheben von UI-Problemen mit <strong>Offscreen-Rendering</strong>, <strong>Mischebenen</strong> und <strong>Rasterung</strong> mithilfe des Tools <strong>Core Animation</strong>, um die Reaktionsfähigkeit des UI-Threads zu erhalten.</td>
<td align="left">Sie können mit einigen einfachen Schritten das XAML-<strong>Markup</strong> und die XAML-<strong>Layouts</strong> <strong>optimieren</strong>. Dazu gehören das Reduzieren der Layoutstruktur, das Minimieren der Elementanzahl und das Minimieren der Überzeichnung. <br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt185403.aspx">Aufrechterhalten der Reaktionsfähigkeit des UI-Threads</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204779.aspx">Optimieren Ihres XAML-Markups</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt404609.aspx">Optimieren des XAML-Layouts</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Threading.</strong> <br><br>Verwenden von Threading zum Aufrechterhalten der <strong>Reaktionsfähigkeit der Benutzeroberfläche</strong> und <strong>parallelen Ausführen mehrerer Aufgaben</strong>.</td>
<td align="left">Threading erfolgt mithilfe der Klassen <strong>Runnable</strong>, <strong>Handler</strong>, <strong>ThreadPoolExecutor</strong> und der abstrakten Klasse <strong>AsyncTask</strong>.</td>
<td align="left">Threading erfolgt mithilfe von <strong>NSThread</strong>, <strong>Grand Central Dispatch</strong> und der abstrakten Klasse <strong>NSOperation</strong>.</td>
<td align="left">Sie können mit Threads arbeiten, indem Sie mit <strong>RunAsync</strong> <strong>Arbeitsaufgaben</strong> an den <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpool.runasync.aspx">Threadpool</a></strong> senden. Sie können mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/br230590.aspx">CreateTimer</a></strong> einen Timer zum Senden einer Arbeitsaufgabe verwenden und mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/br230589.aspx">CreatePeriodicTimer</a></strong> eine regelmäßige Arbeitsaufgabe erstellen.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187339.aspx">Senden einer Arbeitsaufgabe an den Threadpool</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187341.aspx">Senden einer Arbeitsaufgabe mithilfe eines Timers</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187338.aspx">Erstellen einer regelmäßigen Arbeitsaufgabe</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187336.aspx">Bewährte Methoden zum Verwenden des Threadpools</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Asynchrone Programmierung.</strong> <br><br>Vermeiden Sie komplexes Threading, indem Sie mit Mustern der asynchronen Programmierung die Reaktionsfähigkeit des UI-Threads aufrechterhalten.</td>
<td align="left">Zum Erstellen eigener asynchroner Klassen ist <strong>die Verwendung von Threading erforderlich</strong>. Einige integrierte Klassen sind asynchron.</td>
<td align="left">Zum Erstellen eigener asynchroner Klassen ist <strong>die Verwendung von Threading erforderlich</strong>. Einige integrierte Klassen sind asynchron.</td>
<td align="left">Sie können durch die Verwendung asynchroner Muster, z. B. mit <strong>async</strong> und <strong>await</strong> in C# und Visual Basic, vermeiden, dass der Hauptthread blockiert wird, wenn Sie eigene APIs erstellen. Sie können die integrierten asynchronen APIs verwenden, die mit dem Wort <strong>Async</strong> enden.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187335.aspx">Asynchrone Programmierung</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt187337.aspx">Aufrufen asynchroner APIs in C# oder Visual Basic</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Optimierung der Listenansicht.</strong> <br><br>Integrierte Muster zum Optimieren von Datenlisten, die häufig eine geringe Leistung aufweisen, wenn große Mengen von Daten angezeigt werden müssen.</td>
<td align="left">Das <strong>ViewHolder</strong>-Entwurfsmuster wird verwendet, um mehrfache Ansichtssuchvorgänge zu vermeiden, sodass Sie wiederverwendbare UI-Elemente verwenden können.</td>
<td align="left">Die Leistung von <strong>UITableView</strong> kann mit einer Reihe von Optimierungen verbessert werden, es ist jedoch keine Optimierung integriert.</td>
<td align="left">Sie können das <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx">ListView</a>- und das <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx">GridView</a>-Steuerelement verwenden. Diese bieten bereits <strong>UI-Virtualisierung</strong> für gleichmäßiges Schwenken und gleichmäßigen Bildlauf sowie schnelleres Starten. Sie können auch <a href="https://msdn.microsoft.com/library/windows/apps/system.collections.ilist.aspx">IList</a> und <a href="https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged.aspx">INotifyCollectionChanged</a> in der Datenquelle implementieren, um <strong>Datenvirtualisierung</strong> bereitzustellen und die Leistung zusätzlich zu verbessern.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204776.aspx">Optimieren der ListView- und GridView-Benutzeroberfläche</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt574120.aspx">Virtualisierung von ListView- und GridView-Daten</a></td>
</tr>
</tbody>
</table>
<h2 id="monetization">Monetisierung</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>In-App-Käufe.</strong> <br><br>Plattformfeatures ermöglichen es den Benutzern, in Ihren Apps Käufe durchzuführen.</td>
<td align="left"><strong>Eine In-App-Abrechnung</strong> wird über Google-Dienste bereitgestellt. Produkte werden der <strong>Google Play Developer Console</strong> hinzugefügt. In-App-Käufe werden mit der <strong>Google Play Billing Library</strong> implementiert.</td>
<td align="left">Produkte werden <strong>iTunes Connect</strong> hinzugefügt. In-App-Käufe werden mit dem <strong>StoreKit</strong>-Framework implementiert.<br/><br/>Produkte werden mithilfe von <strong>SKMutablePayment</strong> und <strong>SKPaymentQueue</strong> gekauft.</td>
<td align="left">Sie erstellen In-App-Produktkäufe für Ihre App, indem Sie <a href="https://msdn.microsoft.com/library/windows/apps/mt148551.aspx">sie Ihrer App hinzufügen und an den Store übermitteln</a>. <br/><br/>Sie definieren In-App-Käufe mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx">CurrentApp</a></strong>-Klasse. <br/><br/>Mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.requestproductpurchaseasync.aspx">CurrentApp.RequestProductPurchaseAsync</a></strong> zeigen Sie die Benutzeroberfläche an, über die Kunden das Produkt kaufen können.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219684.aspx">Unterstützen von In-App-Produktkäufen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>In-App-Käufe von Verbrauchsartikeln.</strong> <br><br>In-App-Produkte, die gekauft, verwendet und dann erneut gekauft werden können.</td>
<td align="left">Käufe von Verbrauchsartikeln werden aktiviert, indem ein regulärer Kauf getätigt wird und das Produkt dann mit <strong>consumePurchase</strong> verbraucht wird, sodass es gekauft, verwendet und dann erneut gekauft werden kann.</td>
<td align="left">Verbrauchsartikel sind in iTunes Connect <strong>als Verbrauchsartikel definiert</strong>.</td>
<td align="left">Sie unterstützen Verbrauchsartikel, indem Sie <a href="https://msdn.microsoft.com/library/windows/apps/mt148534.aspx">ihren Produkttyp als „Verbrauchsartikel“ definieren, wenn Sie sie an den Store übermitteln</a>. Nach dem Kauf eines Verbrauchartikels rufen Sie <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync.aspx">CurrentApp.ReportConsumableFulfillmentAsync</a></strong> auf, um dem Benutzer den Zugriff auf den Artikel zu ermöglichen.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219683.aspx">Unterstützen von Endverbraucher-In-App-Käufen</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Testen von In-App-Käufen.</strong> <br><br>Ermöglichen es Ihnen, den Code für In-App-Käufe zu testen, ohne Ihre App an den Store zu übermitteln.</td>
<td align="left">Für Tests wird die <strong>In-app Billing Sandbox</strong> verwendet.</td>
<td align="left"><strong>Sandbox-Testerkonten</strong> werden für Tests verwendet.</td>
<td align="left">Zum Testen von In-App-Käufen können Sie einfach die <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx">CurrentAppSimulator</a></strong>-Klasse anstelle von CurrentApp verwenden.<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Testversionen.</strong> <br><br>Ermöglichen es Ihnen, für die Testversion einer App einfach den Inhalt zu beschränken oder Werbung zu entfernen.</td>
<td align="left"><strong>App-Testversionen werden von Google Play nicht offiziell unterstützt</strong>. Für Testversionen oder das Entfernen von Werbung wird ein In-App-Kauf erstellt und der entsprechende Codepfad verwendet, wenn der Kauf erfolgreich bestätigt wurde.</td>
<td align="left"><strong>App-Testversionen werden vom App Store nicht offiziell unterstützt</strong>. Für Testversionen oder das Entfernen von Werbung wird ein In-App-Kauf erstellt und der entsprechende Codepfad verwendet, wenn der Kauf erfolgreich bestätigt wurde.</td>
<td align="left">Sie können eine kostenlose Testversion Ihrer App anbieten, indem Sie beim Übermitteln der App an den Store die <strong><a href="https://msdn.microsoft.com/library/windows/apps/mt148548.aspx">Option „Kostenlose Testversion“</a></strong> verwenden. Anschließend können Sie mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.licenseinformation.istrial.aspx">LicenseInformation.IsTrial</a></strong> den Teststatus der App überprüfen und entsprechend andere Codepfade angeben. Sie können sich für das <a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.licenseinformation.licensechanged">LicenseChanged-Ereignis</a> registrieren, um benachrichtigt zu werden, wenn der Benutzer während der Ausführung der App den Teststatus ändert.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219685.aspx">Ausschließen oder Beschränken von Features in einer Testversion</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">Anpassen an mehrere Plattformen</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Adaptive UI: flexible Layouts.</strong> <br><br>Unterstützen unterschiedlicher Bildschirmgrößen mit flexibler Höhe und Breite.</td>
<td align="left">Flexible Layouts lassen sich mit den Werten <strong>wrap_content</strong> und <strong>match_parent</strong> in LinearLayout-Objekten oder durch Verwendung von RelativeLayout-Objekten für die Ausrichtung erstellen.</td>
<td align="left">Flexible Layouts können unter Verwendung des <strong>adaptiven Modells</strong> mit universellen Storyboards erstellt werden, wobei <strong>Autolayout</strong> mit <strong>Constraints</strong> und <strong>Traits</strong>, z. B. horizontalSizeClass und displayScale, die auf Ansichtscontroller angewendet werden, genutzt wird.</td>
<td align="left">Mit <strong>Layouteigenschaften</strong> und <strong>Panels</strong> sowie einer Kombination von festen und dynamischen Größen erstellen Sie ein dynamisches Layout.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#layout_overview">Definieren von Layouts mit XAML – Layouteigenschaften und -panels</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn958435.aspx">Reaktionsfähiges Design – Grundlagen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Adaptive UI: maßgeschneiderte Layouts.</strong> <br><br>Unterstützung unterschiedlicher Bildschirmgrößen mit eigenen Ziellayouts.</td>
<td align="left">Durch die Bereitstellung alternativer Layoutdateien für unterschiedliche Bildschirmkonfigurationen im Verzeichnis „resources“ mit <strong>Konfigurationsqualifizierern</strong>, z. B. <strong>small</strong>, <strong>large</strong>, <strong>ldpi</strong> und <strong>hdpi</strong>, können Sie benutzerdefinierte Layouts auf Bildschirme unterschiedlicher Größe und Pixeldichte anwenden.</td>
<td align="left">Definieren Sie ein <strong>jeweils eigenes iPhone- und iPad-Storyboard</strong>, um Layouts in einer universellen App an unterschiedliche Gerätefamilien anzupassen.</td>
<td align="left">Sie können ein maßgeschneidertes Layout erstellen, indem Sie <strong>unterschiedliche XAML-Markupdateien</strong> pro Gerätefamilie definieren.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#tailored_layouts">Definieren von Layouts mit XAML – Maßgeschneiderte Layouts</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Adaptive UI: dynamische Layouts.</strong> <br><br>Reaktion auf Änderungen der Bildschirmgröße, z. B. Drehung, oder eine Änderung der Fenstergröße.</td>
<td align="left">Dynamische Layouts werden durch die Verwendung flexibler Layouts mit <strong>LinearLayout</strong> und <strong>RelativeLayout</strong> oder das Bereitstellen alternativer Layoutdateien für unterschiedliche Ausrichtungen ermöglicht.</td>
<td align="left">Wenn sich die <strong>Größe</strong> oder <strong>Traits</strong> einer Ansicht ändern, werden die in Storyboards angegebenen <strong>Constraints</strong> angewendet.</td>
<td align="left">Mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstate.aspx">VisualState</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager.aspx">VisualStateManager</a></strong> und <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.aspx">AdaptiveTrigger</a></strong> können Sie ganz einfach als Reaktion auf Änderungen der Fenstergröße zur Laufzeit Abschnitte der Benutzeroberfläche dynamisch umbrechen, neu positionieren, anzeigen, ersetzen oder ihre Größe ändern.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228350.aspx#visual_states_and_state_triggers">Definieren von Layouts mit XAML – Visuelle Zustände und Zustandsauslöser</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/dn958435.aspx">Reaktionsfähiges Design – Grundlagen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Unterstützen unterschiedlicher Gerätefunktionen.</strong> <br><br>Nutzen Sie erweiterte Hardwarefeatures, während Geräte, die diese nicht aufweisen, trotzdem unterstützt werden.</td>
<td align="left">Durch das Testen von Gerätefeatures zur Laufzeit mit <strong>PackageManager.hasSystemFeature</strong> können Sie bestimmen, ob hardwarespezifischer Code ausgeführt werden kann.</td>
<td align="left">Es gibt <strong>keine einzelne Prüfung</strong>, mit der Sie zur Laufzeit Gerätefeatures testen können. Stattdessen müssen Sie für jedes Feature spezielle Tests ausführen, um zu bestimmen, ob hardwarespezifischer Code ausgeführt werden kann.</td>
<td align="left">Sie können für die Berücksichtigung zusätzlicher Funktionen in unterschiedlichen Gerätefamilien, einschließlich Smartphone, Desktop und IoT, dem Paket <strong>Plattformerweiterungs-SDKs</strong> hinzufügen. Mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx">ApiInformation-API</a></strong> testen Sie zur Laufzeit, ob Typen und Member vorhanden sind, und rufen diese nur auf, wenn dies der Fall ist.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Unterstützen unterschiedlicher Gerätefunktionen.</strong> <br><br>Nutzen Sie erweiterte Hardwarefeatures, während Geräte, die diese nicht aufweisen, trotzdem unterstützt werden.</td>
<td align="left">Die <strong>Android Support Library</strong> kann Ihrer App hinzugefügt werden, um neuere APIs für Geräte verfügbar zu machen, auf denen ältere Versionen von Android ausgeführt werden. Das Testen der API-Ebene zur Laufzeit kann mit <strong>Build.Version.SDK_INT</strong> erfolgen.</td>
<td align="left">Mit Standardlaufzeitprüfungen lässt sich ermitteln, ob APIs verfügbar sind. Zum Beispiel kann mit der <strong>class</strong>-Methode überprüft werden, ob eine Klasse vorhanden ist, und mit <strong>respondsToSelector:</strong> lässt sich überprüfen, ob Methoden in Klassen vorhanden sind.</td>
<td align="left">Mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/dn949005.aspx">ApiInformation.IsApiContractPresent</a></strong> können Sie ermitteln, ob ein API-Vertrag mit einer angegebenen Haupt- und Nebenversionsnummer vorhanden ist. Mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx">ApiInformation-API</a></strong> testen Sie außerdem zur Laufzeit, ob Typen und Member vorhanden sind, und rufen diese nur auf, wenn das der Fall ist.</td>
</tr>
</tbody>
</table>
<h2 id="notifications">Benachrichtigungen</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Kacheln und Signale.</strong> <br><br>Präsentation von Updates für Benutzer auf dem Startbildschirm.</td>
<td align="left"><strong>App-Widgets</strong> sind Ansichten in der Anwendung, die in den Startbildschirm eingebettet und regelmäßige Updates empfangen können. <strong>Ein Signalsystem</strong> ist in Android nicht vorhanden. Es ist kein mit Kacheln identisches System vorhanden.</td>
<td align="left"><strong>Weder Kacheln oder Widgets</strong> werden in iOS unterstützt. Sie können dem Symbol ein <strong>Signal</strong> mit einer Zahl hinzufügen, die sich als Reaktion auf lokale oder Remotebenachrichtigungen ändert.</td>
<td align="left">Ihre App verfügt über eine <strong>Kachel</strong>, die an den Startbildschirm angeheftet werden kann und in der Text und Bilder Ihrer Wahl sowie ein <strong>Signal</strong> mit Glyphen und Zahlen angezeigt werden. Sie können den Inhalt der Kacheln über die App aktualisieren, entweder durch Pushbenachrichtigungen oder zu vordefinierten Zeitpunkten. Kacheln können adaptiv sein und sich abhängig davon ändern, wo sie angezeigt werden.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt185605.aspx">Erstellen von Kacheln</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt590880.aspx">Erstellen adaptiver Kacheln</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187193.aspx">Auswählen einer Methode für die Übermittlung von Benachrichtigungen</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465403.aspx">Richtlinien für Kacheln und Signale</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Anzeigen von Benachrichtigungen.</strong> <br><br>Typen von Benachrichtigungen, die angezeigt werden können.</td>
<td align="left">Benachrichtigungen können im <strong>Benachrichtigungsbereich</strong> und in der <strong>Benachrichtigungsschublade</strong> angezeigt werden. Bei <strong>Heads-Up-Benachrichtigungen</strong> handelt es sich um Benachrichtigungen in einem kleinen unverankerten Fenster. Den Benachrichtigungen können durch Definieren eines <strong>PendingIntent</strong> Aktionen hinzugefügt werden.</td>
<td align="left">Popupbenachrichtigungen werden als <strong>Banner</strong> oder <strong>Warnungen</strong> angezeigt. Sie können <strong>interaktiven Benachrichtigungen</strong>, die mit <strong>UIMutableUserNotificationAction</strong> definiert werden, benutzerdefinierte Aktionsschaltflächen hinzufügen.</td>
<td align="left">Sie können adaptive <strong>Popupbenachrichtigungen</strong> hinzufügen. Sie können in XML Popups mit visuellem Inhalt, mit <strong>Aktionen</strong>, bei denen es sich um Schaltflächen handeln kann, oder mit Eingaben und Audioinhalten definieren.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt631604.aspx">Adaptive und interaktive Popupbenachrichtigungen</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187193.aspx">Auswählen einer Methode für die Übermittlung von Benachrichtigungen</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465391.aspx">Richtlinien für Popupbenachrichtigungen</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Planen von lokalen Benachrichtigungen.</strong> <br><br>Lokale Benachrichtigungen, die von der App zu einem geplanten Zeitpunkt gesendet werden.</td>
<td align="left">Benachrichtigungen und Aktionen werden mit einem <strong>NotificationCompat.Builder</strong> definiert. Sie können mit <strong>AlarmManager</strong> und <strong>BroadcastReceiver</strong> in der App geplant und behandelt werden.</td>
<td align="left">Lokale Benachrichtigungen werden mit <strong>UILocalNotification</strong> erstellt und können mit **UILocalNotification.scheduleLocalNotification:<strong> geplant werden. Sie können Popupbenachrichtigungen mit </strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.scheduledtoastnotification.aspx">ScheduledToastNotification</a><strong> planen. Sie können mit der </strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.tilenotification.aspx">TileNotification-Klasse</a><strong> eine Kachelbenachrichtigung von der App senden oder mit <a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.notifications.scheduledtilenotification.aspx">ScheduledTileNotification</a> eine Kachelbenachrichtigung planen.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt631604.aspx">Adaptive und interaktive Popupbenachrichtigungen</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt593299.aspx">Senden einer lokalen Kachelbenachrichtigung</a> || </strong>Senden von Pushbenachrichtigungen** Von einem Pushbenachrichtigungsserver wird eine Benachrichtigung gesendet, die optional in der App behandelt wird.</td>
<td align="left"><strong>Google Cloud Messaging</strong> unterstützt Pushbenachrichtigungen für Android.</td>
</tr>
</tbody>
</table>
<h2 id="media-capture-and-rendering">Medienaufzeichnung und -rendering</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Aufzeichnen von Medien.</strong> <br><br>Aufzeichnen von Audio- und visuellen Inhalten.</td>
<td align="left">Mithilfe eines <strong>Intents</strong>, z. B. MediaStore.ACTION_VIDEO_CAPTURE, können Medien mit einer vorhandenen Kamera-App aufgezeichnet werden. Die Bibliothek <strong>android.hardware.camera2</strong> oder <strong>camera</strong> ermöglicht die Implementierung einer benutzerdefinierten Kameraschnittstelle. <strong>MediaRecorder</strong>-APIs können für die Audioaufzeichnung verwendet werden.</td>
<td align="left">Der <strong>UIImagePickerController</strong> ermöglicht das Aufzeichnen von Videos und Fotos mit der Benutzeroberfläche des Systems. Die <strong>AVFoundation</strong>-Klassen, z. B. <strong>AVCaptureSession</strong>, aktivieren den direkten Zugriff auf die Kamera. <br/>Die <strong>AVAudioRecorder</strong>-Klasse ermöglicht die Audioaufzeichnung.</td>
<td align="left">Mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.cameracaptureui.aspx">CameraCaptureUI</a></strong>-Klasse können Sie Fotos und Videos mit der integrierten Kamera-UI aufzeichnen. Sie können auf einer niedrigen Ebene mit der Kamera interagieren und Klassen in <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.aspx">Windows.Media.Capture</a></strong>, beispielsweise die <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.capture.mediacapture.aspx">MediaCapture-API</a></strong> für die Audioaufzeichnung verwenden. <br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt282142.aspx">Aufnehmen von Fotos und Videos mit „CameraCaptureUI“</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243896.aspx">Aufnehmen von Fotos und Videos mit „MediaCapture“</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Medienwiedergabe.</strong> <br><br>Wiedergeben von Audio- und Videodateien.</td>
<td align="left">Zum Wiedergeben von Audio- und Videodateien werden die <strong>MediaPlayer</strong>-Klasse und die <strong>AudioManager</strong>-Klasse verwendet.</td>
<td align="left">Zum Wiedergeben von Audio- und Videodateien werden das <strong>AVKit-Framework</strong>, <strong>AVAudioPlayer</strong> und das <strong>Media Player-Framework</strong> verwendet.</td>
<td align="left">Sie können mit den Klassen <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.media.core.mediasource.aspx">MediaSource</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx">MediaElement</a></strong> und <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx">MediaPlayer</a></strong> Audio- und Videodateien aus Quellen, z. B. lokalen und Remotedateien, wiedergeben.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt592657.aspx">Medienwiedergabe mit „MediaSource“</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Bearbeiten von Medien.</strong> <br><br>Erstellen neuer Mediendateien aus vorhandenen Aufzeichnungen und Anwenden von Spezialeffekten.</td>
<td align="left">Zum Bearbeiten von Inhalten können Klassen niedriger Ebene, z. B. <strong>MediaCodec</strong>, <strong>MediaMuxer</strong> und <strong>android.media.effect</strong>, verwendet werden.</td>
<td align="left">Zum Bearbeiten von Inhalten können Klassen im <strong>AV Foundation</strong>-Framework, z. B. <strong>AVMutableComposition</strong>, <strong>AVMutableVideoComposition</strong> und <strong>AVMutableAudioMix</strong>, verwendet werden.</td>
<td align="left">Sie können mit den <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.aspx">Windows.Media.Editing</a></strong>-APIs, z. B. <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.mediacomposition.aspx">MediaComposition</a></strong> und <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.media.editing.mediaclip.aspx">MediaClip</a></strong>, Medienkompositionen aus Audio- und Videodateien erstellen. Sie können Video- und Bildüberlagerungen hinzufügen, Videoclips kombinieren, Hintergrundaudio hinzufügen und Audio- und Videoeffekte anwenden.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt204792.aspx">Medienkompositionen und -bearbeitung</a></td>
</tr>
</tbody>
</table>
<h2 id="sensors">Sensoren</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Sensoren.</strong> <br><br>Erkennen der Bewegung, Position und Umwelteigenschaften des Geräts.</td>
<td align="left">Das <strong>Sensor-Framework</strong> wird für den Zugriff auf Hardware- und Softwaresensoren mit Klassen, z. B. <strong>SensorManager</strong> und <strong>SensorEvent</strong>, verwendet.</td>
<td align="left">Das <strong>Core Motion Framework</strong> wird für den Zugriff auf Sensorrohdaten und verarbeitete Sensordaten verwendet.</td>
<td align="left">Sie können Klassen in <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.aspx">Windows.Devices.Sensors</a></strong> für den Zugriff auf Sensorwerte und Ereignisse verwenden, die ausgelöst werden, wenn der Sensor neue Messdaten empfängt.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt187358.aspx">Sensoren</a></td>
</tr>
</tbody>
</table>
<h2 id="location-and-mapping">Position und Kartenfunktionen</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Position.</strong> <br><br>Suchen der <strong>aktuellen</strong> Position des Geräts und Nachverfolgen von <strong>Änderungen</strong>.</td>
<td align="left">Die Standort-APIs der Google Play-Dienste bieten mit dem <strong>Fused Location Provider</strong> unter Verwendung der <strong>getLastLocation</strong>-Methode und der <strong>requestLocationUpdates</strong>-Methode High-Level-Zugriff auf die <strong>letzte bekannte Position</strong>. Low-Level-Zugriff wird in den Android-Bibliotheken mit dem <strong>LocationManager</strong> bereitgestellt.</td>
<td align="left">Die <strong>Core Location</strong> <strong>CLLocationManager</strong>-Klasse wird zum Überwachen der Position eines Geräts verwendet, mit <strong>startUpdatingLocation</strong> für den Standardstandortdienst und <strong>startMonitoringSignificantLocationChanges</strong> für den <strong>significant-change</strong>-Standortdienst.</td>
<td align="left">Sie können die Geräteposition mit Klassen in <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.aspx">Windows.Devices.Geolocation</a></strong> nachverfolgen. Verwenden Sie für einmaliges Lesen <strong><a href="https://msdn.microsoft.com/library/windows/apps/br225537.aspx">Geolocator.GetGeopositionAsync</a></strong>. Verwenden Sie <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.positionchanged.aspx">Geolocator.PositionChanged</a></strong>, um die Position mithilfe eines Timers regelmäßig abzurufen oder um informiert zu werden, wenn sich die Position ändert.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219698.aspx">Abrufen der Position eines Benutzers</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Anzeigen von Karten.</strong> <br><br>Anzeigen einer <strong>interaktiven integrierten Karte</strong> und Hinzufügen von <strong>interessanten Orten</strong>.</td>
<td align="left">Die Klassen <strong>GoogleMap</strong>, <strong>MapFragment</strong> und <strong>MapView</strong> in der <strong>Google Maps Android API</strong> ermöglichen das Einbetten von Karten in Apps. Interessante Orte können mit <strong>Markern</strong> und der anpassbaren <strong>Marker</strong>-Klasse angezeigt werden.</td>
<td align="left">Karten werden mit der <strong>MKMapView</strong>-Klasse im <strong>MapKit-Framework</strong> in iOS-Apps eingebettet. <strong>Es können Anmerkungen</strong> mit Objektklassen, z. B. <strong>MKPointAnnotation</strong> und <strong>MKPinAnnotationView</strong>, zu den Apps hinzugefügt werden, um interessante Orte anzuzeigen.</td>
<td align="left">Sie können mit dem integrierten <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx">MapControl</a></strong>-XAML-Steuerelement, das 2D-, 3D- und Streetside-Ansichten bereitstellt, Karten in die Apps einbetten. Mit Klassen wie <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapicon.aspx">MapIcon</a></strong>, <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mappolygon.aspx">MapPolygon</a></strong> und <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mappolyline.aspx">MapPolyline</a></strong> können Sie interessante Orte mit einer Ortsmarke, einem Bild oder einer Form hinzufügen.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219695.aspx">Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219696.aspx">Anzeigen von interessanten Orten (POI) auf einer Karte</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Geofencing.</strong> <br><br>Überwachen des Betretens und Verlassens einer bestimmten geografischen Region.</td>
<td align="left">Geofences werden mit den <strong>Location Services</strong> im Google Play Services SDK überwacht.</td>
<td align="left">Regionen werden mit der <strong>CLCircularRegion</strong>-Klasse überwacht und mit der <strong>CLLocationManager.startMonitoringForRegion:</strong>-Methode registriert.</td>
<td align="left">Sie können einen Geofence-Bereich mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geofencing.geofence.aspx">Geofence</a></strong>-Klasse erstellen und <strong>überwachte Zustände</strong>, z. B. das Betreten oder Verlassen einer Region, definieren. Mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geofencing.geofencemonitor.aspx">GeofenceMonitor-Klasse</a></strong> behandeln Sie Geofence-Ereignisse im Vordergrund und mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.locationtrigger.aspx">LocationTrigger-Hintergrundklasse</a></strong> im Hintergrund.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219702.aspx">Einrichten von Geofence-Bereichen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Geocodierung und umgekehrte Geocodierung.</strong> <br><br>Das Umwandeln von Adressen in geografische Standorte (Geocodierung) und das Umwandeln geografischer Standorte in Adressen (umgekehrte Geocodierung).<br/></td>
<td align="left">Für Geocodierung und umgekehrte Geocodierung wird die <strong>Geocoder</strong>-Klasse verwendet.</td>
<td align="left">Für Geocodierung wird die <strong>CLGeocoder</strong>-Klasse verwendet.</td>
<td align="left">Sie können Geocodierung mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx">MapLocationFinder</a></strong>-Klasse in <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.aspx">Windows.Services.Maps</a></strong> ausführen. Sie verwenden <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.findlocationsasync.aspx">FindLocationsAsync</a></strong> für Geocodierung und <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.findlocationsatasync.aspx">FindLocationsAtAsync</a></strong> für umgekehrte Geocodierung.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219697.aspx">Durchführen der Geocodierung und umgekehrter Geocodierung</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Routen und Wegbeschreibungen.</strong> <br><br>Bereitstellen von Routen und Entfernungen zwischen zwei geografische Standorten und der entsprechenden Wegbeschreibungen.</td>
<td align="left">Google bietet den Webdienst <strong>Google Maps Directions API</strong>, der in Android verwendet werden kann, obwohl kein SDK verfügbar ist.</td>
<td align="left">MapKit stellt die <strong>MKDirections</strong>-API bereit, mit der Informationen zu einer Route und Wegbeschreibungen abgerufen werden können.</td>
<td align="left">Sie können mit der <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproutefinder.aspx">MapRouteFinder</a></strong>-Klasse in <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.aspx">Windows.Services.Maps</a></strong> eine Fußgänger -oder Autoroute anfordern. Routen werden als <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproute.aspx">MapRoute</a></strong>-Instanz zurückgegeben, die einfach in einem MapControl angezeigt werden kann. Wegbeschreibungen werden im <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maproutemaneuver.aspx">MapRouteManeuver</a></strong> -Objekt zurückgegeben.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt219701.aspx">Anzeigen von Routen und Wegbeschreibungen auf einer Karte</a></td>
</tr>
</tbody>
</table>
<h2 id="app-to-app-communication">App-zu-App-Kommunikation</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Aufrufen einer anderen App.</strong> <br><br>Starten einer anderen App und optionales Freigeben von Daten, z. B. Links, Text, Fotos, Videos und Dateien.</td>
<td align="left">Zum Starten einer anderen App wird ein <strong>impliziter Intent</strong> verwendet. Dabei werden eine <strong>Action</strong> und optionale Daten in einem <strong>Intent</strong> definiert und mit <strong>startActivityForResult</strong> aufgerufen.<br/></td>
<td align="left"><strong>App-Erweiterungen</strong> können verwendet werden, um Zugriff auf App-Daten für eine andere App bereitzustellen. <strong>URL-Schemas</strong> ermöglichen das Übergeben einer URL an eine andere App.</td>
<td align="left">Sie können mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.launcher.launchuriasync.aspx">Launcher.LaunchUriAsync</a></strong> eine andere App aufrufen, die sich für einen URI registriert hat, oder <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.system.launcher.launchuriforresultsasync.aspx">Launcher.LaunchUriForResultsAsync</a></strong> verwenden, um eine andere App aufzurufen und Ergebnisse von dieser abzurufen. Sie können mit <strong><a href="https://msdn.microsoft.com/library/windows/apps/hh701471.aspx">Launcher.LaunchFileAsync</a></strong> eine Datei an eine andere App übergeben, damit sie von dieser behandelt wird.<br/><br/>Zum Freigeben von Daten zwischen Apps können Sie einfach einen <strong>Freigabe-Vertrag</strong> verwenden.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt228340.aspx">Starten der Standard-App für einen URI</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269386.aspx">Starten einer App für Ergebnisse</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt299102.aspx">Starten der Standard-App für eine Datei</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243293.aspx">Freigeben von Daten</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Zulassen des Aufrufs der App.</strong> <br><br>Zulassen, dass Ihre App auf eine Anforderung von einer anderen App antwortet.</td>
<td align="left">Apps registrieren eine <strong>Intent-Behandlungsaktivität</strong> bei einem <strong>Intent-Filter</strong>, um auf einen impliziten Intent von einer anderen App zu reagieren.</td>
<td align="left">Durch das Verpacken einer <strong>App-Erweiterung</strong> können Daten für andere Apps freigegeben werden. Apps können mit dem Schlüssel <strong>CFBundleURLTypes</strong> in „Info.plist“ ein <strong>benutzerdefiniertes URL-Schema</strong> registrieren.</td>
<td align="left">Sie können Ihre App als Standardhandler für einen <strong>URI-Schemanamen</strong> registrieren, indem Sie ein <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.activationkind.aspx#Protocol">Protokoll</a></strong> im Paketmanifest registrieren und den <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx">Application.OnActivated</a></strong> Ereignishandler aktualisieren. Optional werden Ergebnisse zurückgegeben. Auf die gleiche Weise können Sie Ihre App als Standardhandler für bestimmte Dateitypen registrieren, indem Sie im Paketmanifest eine Deklaration hinzufügen und das <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onfileactivated.aspx">Application.OnFileActivated</a></strong>-Ereignis behandeln.<br/><br/>Sie können die Freigabe-Vertrag-Anforderungen behandeln, indem Sie die App im Manifest als Freigabeziel registrieren und das <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onsharetargetactivated.aspx">Application.OnShareTargetActivated</a></strong>-Ereignis behandeln.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269386.aspx">Starten einer App für Ergebnisse</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/mt269385.aspx">Behandeln der Dateiaktivierung</a><br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243292.aspx">Empfangen von Daten</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Kopieren und Einfügen.</strong> <br><br>Kopieren und Einfügen von Text und anderen Inhalten zwischen Apps.</td>
<td align="left">Das <strong>Clipboard Framework</strong> kann verwendet werden, um Kopieren und Einfügen mit der <strong>ClipboardManager</strong>-Klasse und der <strong>ClipData</strong>-Klasse zu implementieren.</td>
<td align="left">Zum Implementieren von Kopieren und Einfügen können <strong>UIPasteboard</strong>, <strong>UIMenuController</strong> und <strong>UIResponderStandardEditActions</strong> verwendet werden.</td>
<td align="left">Viele Standard-XAML-Steuerelemente unterstützen bereits Kopieren und Einfügen. Sie können mithilfe der Klassen <strong><a href="https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx">DataPackage</a></strong> und <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.aspx">Clipboard</a></strong> in <strong><a href="https://msdn.microsoft.com/library/windows/apps/br205967">Windows.ApplicationModel.DataTransfer</a></strong> das Kopieren und Einfügen selbst implementieren.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt243291.aspx">Kopieren und Einfügen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Drag & Drop.</strong> <br><br>Drag & Drop von Inhalten zwischen Apps.</td>
<td align="left">Drag & Drop kann mit dem <strong>Android Drag/Drop Framework</strong> in einer einzelnen Anwendung implementiert werden.</td>
<td align="left">Von iOS werden keine allgemeinen Drag & Drop-APIs bereitgestellt.</td>
<td align="left">Sie können in der App Drag & Drop implementieren, um Drag & Drop-Funktionen von App zu App, von Desktop zu App und von App zu Desktop zu ermöglichen. Sie können in der UIElement-Klasse mit den Eigenschaften <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx">AllowDrop</a></strong> und <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx">CanDrag</a></strong> und den Ereignissen <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx">DragOver</a></strong>, und <strong><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx">Drop</a></strong> Unterstützung für Drag & Drop implementieren.<br/><br/><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt227651.aspx">Drag & Drop</a></td>
</tr>
</tbody>
</table>
<h2 id="software-design">Softwaredesign</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Softwaredesignmuster.</strong> <br><br>Empfohlene oder perfekt verwendete Muster für die Plattform.</td>
<td align="left">Für die Android-Entwicklung wurde kein formales Musters empfohlen oder bereitgestellt, jedoch ermöglicht eventuell die Betaversion des Data Binding Frameworks die häufigere Verwendung des <strong>Model-View-ViewModel (MVVM)</strong>-Musters. In einer Reihe von Drittanbieterartikeln und -Frameworks werden die Ansätze <strong>Model-View-Presenter (MVP)</strong> und <strong>MVVM</strong> empfohlen.</td>
<td align="left"><strong>Model-View-Controller (MVC)</strong> ist ein häufig verwendetes Muster für iOS und in die Plattform integriert.</td>
<td align="left">Sie sind beim Softwaredesign für UWP nicht auf ein bestimmtes Muster beschränkt.<br/><br/>Sie können das integrierte <a href="https://msdn.microsoft.com/library/windows/apps/mt210947.aspx">Datenbindungsmuster</a> verwenden, um eine deutliche Trennung von Datenaspekten und UI-Aspekten sicherzustellen und zu vermeiden, dass Sie UI-Ereignishandler programmieren müssen, die Eigenschaftswerte aktualisieren.<br/><br/>Sie können die Datenbindung auf die Befolgung des <strong>Model-View-ViewModel (MVVM)</strong>-Musters erweitern, indem Sie MVVM-Bibliotheken von Drittanbietern, z. B. das <a href="https://mvvmlight.codeplex.com/">MVVM Light Toolkit</a>, nutzen, oder eine eigene Bibliothek verwenden und Logik von der CodeBehind-Datei fernhalten.<br/><br/><a href="https://msdn.microsoft.com/library/hh848246.aspx">Das MVVM-Muster</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">Visual Studio-Template 10-Projektvorlagen</a></td>
</tr>
</tbody>
</table>



<!--HONumber=Jun16_HO4-->


