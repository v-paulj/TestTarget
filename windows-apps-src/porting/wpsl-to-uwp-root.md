---
description: Entwickler, die eine Windows Phone Silverlight-App haben, können beim Wechsel zu Windows 10 auf Ihre Kenntnisse zurückgreifen und vorhandenen Quellcode nutzen.
title: Wechsel von Windows Phone Silverlight zu UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
---

#  Wechsel von Windows Phone Silverlight zu UWP

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Entwickler, die über eine Windows Phone Silverlight-App verfügen, können beim Wechsel zu Windows 10 auf Ihre Kenntnisse und Fähigkeiten zurückgreifen und Ihren vorhandenen Quellcode umfassend nutzen. Mit Windows 10 können Sie eine UWP (Universelle Windows-Plattform)-App erstellen, also ein einzelnes App-Paket, das Ihre Kunden auf jeder Art von Gerät installieren können. Unter [Anleitung für UWP (Universelle Windows-Plattform)-Apps](https://msdn.microsoft.com/library/windows/apps/dn894631)finden Sie weitere Hintergrundinformationen zu Windows 10, UWP-Apps und den Konzepten von adaptivem Code und adaptiver UI, die in diesem Portierungshandbuch behandelt werden.

Wenn Sie Ihre Windows Phone Silverlight-App zu einer Windows 10-App portieren, können Sie den neuesten Stand für die Mobilgerätfeatures herstellen, die [mit Windows Phone 8.1 eingeführt](https://msdn.microsoft.com/library/windows/apps/dn632424) wurden. Außerdem können Sie einen großen Schritt darüber hinaus machen, indem Sie die universelle Windows-Plattform (UWP) nutzen, deren App-Modell und Benutzeroberflächenframework universell für alle Windows 10-Geräte gelten. Dies ermöglicht die Unterstützung von PCs, Tablets, Telefonen und einer großen Zahl von anderen Gerätearten – mit einer Codebasis und einem App-Paket. Zudem vervielfacht sich die potenzielle Zielgruppe Ihrer App, und Sie erhalten neue Möglichkeiten durch freigegebene Daten, gekaufte Consumables usw. Weitere Informationen zu den neuen Features finden Sie unter [Neues bei Windows 10 für Entwickler](https://dev.windows.com/getstarted/whats-new-windows-10).

Bei Bedarf können Sie die Windows Phone Silverlight-Version Ihrer App und die Windows 10-Version gleichzeitig für Kunden verfügbar machen.

**Hinweis**  Dieser Leitfaden soll Sie beim manuellen Portieren Ihrer Windows Phone Silverlight-App auf Windows 10 unterstützen. Zum Portieren Ihrer App können Sie neben den Informationen in diesem Leitfaden auch die Developer Preview der **Silverlight-Brücke von Mobilize.Net** ausprobieren, um den Portierungsprozess zu automatisieren. Dieses Tool analysiert den Quellcode Ihrer App und konvertiert Verweise auf Windows Phone Silverlight-Steuerelemente und -APIs in ihre UWP-Entsprechungen. Da dieses Tool noch die Developer Preview-Version aufweist, unterstützt es noch nicht alle Konvertierungsszenarien. Die meisten Entwickler sollten mit dem Einstieg in dieses Tool jedoch Zeit und Mühe sparen. Besuchen Sie die [Website von Mobilize.NET](http://go.microsoft.com/fwlink/p/?LinkId=624546), um die Developer Preview zu testen.

## XAML und .NET oder HTML?

Windows Phone Silverlight verfügt über ein XAML-Benutzeroberflächenframework, das auf Silverlight 4.0 basiert, und Sie programmieren mit einer Version von .NET Framework sowie einer kleinen Teilmenge von UWP-APIs. Da Sie Extensible Application Markup Language (XAML) in Ihrer Windows Phone Silverlight-App verwendet haben, werden Sie sich für Ihre Windows 10-Version wahrscheinlich für XAML entscheiden. Ein Großteil Ihres Wissens und Ihrer Erfahrung sind übertragbar, und dies gilt auch für Ihren Quellcode und die verwendeten Softwaremuster. Auch Ihr UI-Markup und -Design lassen sich mühelos portieren. Ihnen werden die verwalteten APIs, das XAML-Markup, das Benutzeroberflächenframework und die Tools angenehm vertraut vorkommen, und Sie können C++, C# oder Visual Basic zusammen mit XAML in einer UWP-App verwenden. Obwohl es die eine oder andere Herausforderung zu meistern gilt, werden Sie vielleicht überrascht sein, wie einfach der Prozess letztlich ist.

Siehe [Roadmap für Universal Windows Platform (UWP)-Apps mit C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/br229583).

**Hinweis**  Windows 10 unterstützt einen deutlich größeren Teil von .NET Framework als eine Windows Phone Store-App. Windows 10 verfügt beispielsweise über mehrere System.ServiceModel.\*-Namespaces sowie über System.Net, System.Net.NetworkInformation und System.Net.Sockets. Der Zeitpunkt für das Portieren von Windows Phone Silverlight ist also günstig, da Sie Ihren .NET-Code für die Ausführung auf der neuen Plattform einfach kompilieren können. Weitere Informationen finden Sie unter [Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md).
Ein weiterer guter Grund zum Neukompilieren Ihres vorhandenen .NET-Quellcodes in eine Windows 10-App ist, dass Sie von .NET Native profitieren. Dabei handelt es sich um eine fortschrittliche Kompilierungstechnologie, mit der MSIL-Code in Computercode für die systemeigene Ausführung konvertiert wird. .NET Native-Apps starten schneller, verbrauchen weniger Arbeitsspeicher und benötigen weniger Akkuenergie als ihre MSIL-Gegenstücke.

In diesem Portierungshandbuch geht es vorrangig um XAML. Alternativ können Sie mit JavaScript, Cascading Stylesheets (CSS) und HTML5 sowie der Windows-Bibliothek für JavaScript eine funktional vergleichbare App erstellen – viele der verwendeten UWP-APIs sind in diesem Fall identisch. Obwohl sich die Windows-Runtime-Benutzeroberflächenframeworks von XAML und HTML unterscheiden, funktionieren beide für sämtliche Windows-Geräte.

## Universelle Geräte oder Mobilgeräte

Eine Option ist das Portieren Ihrer App zu einer App für universelle Geräte. In diesem Fall besteht für die Installation der App der größte Spielraum. Wenn Ihre App die APIs aufruft, die nur in Mobilgeräten implementiert sind, können Sie diese Aufrufe mit adaptivem Code schützen. Alternativ dazu können Sie Ihre App auch zu einer App für Mobilgeräte portieren. In diesem Fall müssen Sie keinen adaptiven Code schreiben.

## Anpassen der App an mehrere Formfaktoren

Die im vorherigen Abschnitt gewählte Option bestimmt die Palette an Geräten, auf denen Ihre App oder Apps ausgeführt werden, und dabei kann es sich um eine sehr breite Palette von Geräten handeln. Selbst wenn Sie Ihre App auf Mobilgeräte beschränken, müssen Sie dennoch ein breites Spektrum an Bildschirmgrößen unterstützen. Wenn Ihre App also auf verschiedenen Formfaktoren ausgeführt wird, die vorher nicht unterstützt wurden, sollten Sie Ihre Benutzeroberfläche auf diesen Formfaktoren testen und ggf. Änderungen vornehmen, damit sich die UI auf jedem Gerät entsprechend anpasst. Stellen Sie sich dies als eine Aufgabe nach der Portierung oder eine erweiterte Zielvorgabe für das Portieren vor. Die Fallstudie [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) enthält hierzu ein Praxisbeispiel.

## Portieren der einzelnen Ebenen

-   **Ansicht**. Die Ansicht und das Ansichtsmodell bilden die UI Ihrer App. Im Idealfall besteht die Ansicht aus Markup, das an feststellbare Eigenschaften eines Ansichtsmodells gebunden ist. Ein weiteres gängiges, aber nur auf kurze Sicht zweckmäßiges Muster ist das direkte Ändern von UI-Elementen mit imperativem Code in einer CodeBehind-Datei. In beiden Fällen kann ein Großteil des UI-Markups und -Designs – und sogar imperativer Code zum Ändern von UI-Elementen – einfach portiert werden.
-   **Ansichtsmodelle und Datenmodelle**. Auch wenn Sie Muster zur Trennung der Zuständigkeiten (z. B. MVVM) nicht ausdrücklich anwenden, gibt es in Ihrer App zwangsläufig Code, der die Funktion des Ansichts- und Datenmodells übernimmt. Code für das Ansichtsmodell nutzt Typen in den Namespaces des Benutzeroberflächenframeworks. Sowohl der Code für das Ansichtsmodell als auch der Code für das Datenmodell nutzen zudem nicht visuelle Betriebssystem- und .NET-APIs (darunter APIs für den Datenzugriff). Und die meisten dieser APIs sind [für UWP-Apps verfügbar](https://msdn.microsoft.com/library/windows/apps/br211369). Sie können daher davon ausgehen, dass sich ein Großteil dieses Codes ohne Änderungen portieren lässt. Bedenken Sie aber, dass ein Ansichtsmodell ein Modell oder eine *Abstraktion* einer Ansicht ist. Ein Ansichtsmodell stellt den Zustand und das Verhalten der UI bereit, während die Ansicht selbst die visuellen Elemente bereitstellt. Aus diesem Grund sind wahrscheinlich für jede UI, die Sie an die verschiedenen, von der UWP unterstützten Formfaktoren anpassen, entsprechende Änderungen des Ansichtsmodells erforderlich. Für Netzwerke und das Aufrufen von Clouddiensten können Sie normalerweise zwischen .NET und UWP-APIs wählen. Informationen zu den Faktoren, die diese Entscheidung beeinflussen, finden Sie unter [Clouddienste, Netzwerk und Datenbanken](wpsl-to-uwp-business-and-data.md#networking-cloud).
-   .**Clouddienste** Höchstwahrscheinlich werden einige (oder sogar die meisten) Teile Ihrer App in Form von Diensten in der Cloud ausgeführt. Der auf dem Clientgerät ausgeführte Teil der App stellt eine Verbindung mit diesen Diensten her. Dies ist der Teil einer verteilten App, bei dem es am wahrscheinlichsten ist, dass er beim Portieren des Clientteils unverändert bleibt. Falls Sie noch keine Clouddienste nutzen, sind [Microsoft Azure Mobile Services](http://azure.microsoft.com/services/mobile-services/) eine gute Wahl für Ihre UWP-App. Sie bieten leistungsstarke Back-End-Komponenten, die universelle Windows-Apps für verschiedenste Dienste aufrufen können – angefangen bei einfachen Benachrichtigungen für Live-Kachelaktualisierungen bis hin zur komplexen Skalierbarkeit, die eine Serverfarm bereitstellen kann.

Überlegen Sie vor oder während der Portierung, ob Ihre App dadurch verbessert werden könnte, wenn Sie sie umgestalten und Code mit einem ähnlichen Zweck in Ebenen zusammenfassen, anstatt ihn willkürlich zu verteilen. Die Aufteilung Ihrer UWP-App in die oben beschriebenen Ebenen erleichtert Ihnen das Ausschließen von Fehlern, das Testen sowie das spätere Lesen und Warten Ihrer App. Sie können die Wiederverwendbarkeit von Funktionen verbessern – und Probleme mit Unterschieden in den UI-APIs vermeiden –, indem Sie das Model-View-ViewModel-Muster ([MVVM](http://msdn.microsoft.com/magazine/dd419663.aspx)) anwenden. Dieses Muster trennt die Daten-, Geschäfts- und UI-Teile der App voneinander. Auch innerhalb der UI kann das Muster Zustand und Verhalten von den visuellen Elementen getrennt halten, sodass diese separat getestet werden können. Das MVVM-Muster bietet Ihnen die Möglichkeit, Ihre Daten und Geschäftslogik einmal zu schreiben und auf allen Geräten, unabhängig von der UI, zu verwenden. Es ist wahrscheinlich, dass Sie auch einen Großteil des Ansichtsmodells und der Ansichtselemente auf verschiedenen Geräten wiederverwenden können.

## Ein oder zwei Ausnahmen von der Regel

Beim Lesen dieses Portierungshandbuchs können Sie auch die Informationen unter [Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md) nutzen. Im Allgemeinen wird eine einfache Zuordnung verwendet, und in der Tabelle mit den Namespace- und Klassenzuordnungen werden etwaige Ausnahmen beschrieben.

Die gute Nachricht ist, dass auf der Featureebene nur sehr wenig nicht in der UWP unterstützt wird. Wenn Sie den Rest dieses Handbuchs für das Portieren lesen, werden Sie feststellen, dass sich die meisten Ihrer Kenntnisse und der Großteil Ihres Quellcodes sehr gut auf UWP-Apps übertragen lassen. In der folgenden Tabelle sind die wenigen Windows Phone Silverlight-Features aufgeführt, die Sie vielleicht verwendet haben und für die es keine Entsprechung in der UWP gibt.

| Feature, für das es kein UWP-Äquivalent gibt | Windows Phone Silverlight-Dokumentation für das Feature |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA. Im Allgemeinen wird als Ersatz [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) mit C++ verwendet. Siehe [Entwickeln von Spielen](https://msdn.microsoft.com/library/windows/apps/hh452744) und [Interoperabilität von DirectX und XAML](https://msdn.microsoft.com/library/windows/apps/hh825871). | [XNA-Framework-Klassenbibliothek](http://msdn.microsoft.com/library/bb203940.aspx) | 
|Foto-Apps | [Objektive für Windows Phone 8](http://msdn.microsoft.com/library/windows/apps/jj206990.aspx) |

&nbsp;

| Thema| Beschreibung|
|------|------------| 
| [Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md) | Dieses Thema bietet eine umfassende Zuordnung der Windows Phone Silverlight-APIs in ihren UWP-Äquivalenten. |
| [Portieren des Projekts](wpsl-to-uwp-porting-to-a-uwp-project.md) | Sie beginnen den Portierungsprozess, indem Sie zunächst in Visual Studio ein neues Windows 10-Projekt erstellen und Ihre Dateien in das Projekt kopieren. |
| [Fehlerbehebung](wpsl-to-uwp-troubleshooting.md) | Wir empfehlen dringend, dieses Handbuch für das Portieren vollständig zu lesen. Wir wissen aber auch, dass Sie möglichst schnell die Phase erreichen möchten, in der Ihr Projekt erstellt und ausgeführt wird. Zu diesem Zweck können Sie einen temporären Fortschritt erzielen, indem Sie allen nicht unbedingt erforderlichen Code auskommentieren und anschließend zurückkehren, um die Schulden später zu bezahlen. Die Tabelle mit Symptomen und Möglichkeiten zur Problembehandlung in diesem Thema kann in dieser Phase hilfreich für Sie sein, ersetzt jedoch nicht das Lesen der nächsten Themen. Sie können die Tabelle jederzeit zu Rate ziehen, während Sie die weiteren Themen lesen. |
| [Portieren von XAML und UI](wpsl-to-uwp-porting-xaml-and-ui.md) | Die Vorgehensweise zum Definieren einer UI in Form von deklarativem XAML-Markup lässt sich sehr gut von Windows Phone Silverlight- auf UWP-Apps übertragen. Sie werden feststellen, dass große Abschnitte Ihres Markups kompatibel sind, sobald Sie Verweise auf Systemressourcenschlüssel aktualisiert, einige Namen von Elementtypen geändert und „clr-namespace“ in „using“ geändert haben. |
| [Portieren: E/A, Gerät und App-Modell](wpsl-to-uwp-input-and-sensors.md) | Code, der in das Gerät selbst integriert und auf dessen Sensoren abgestimmt ist, umfasst auch Eingaben vom und Ausgaben an den Benutzer. Auch die Datenverarbeitung kann einbezogen werden. Aber dieser Code wird in der Regel nicht als UI-Ebene oder Datenebene betrachtet. Dieser Code enthält die Integration in Vibrationscontroller, Beschleunigungsmesser, Gyroskop, Mikrofon und Lautsprecher (überschneiden sich mit Spracherkennung und Sprachsynthese), (geografischen) Standort und Eingabemodalitäten, z. B. Touch, Maus, Tastatur und Stift. |
| [Portieren der Geschäfts- und Datenebene](wpsl-to-uwp-business-and-data.md) | Im Hintergrund Ihrer UI befinden sich Ihre Geschäfts- und Datenebenen. Der Code auf diesen Ebenen ruft Betriebssystem- und .NET Framework-APIs auf (z. B. Hintergrundverarbeitung, Position, Kamera, Dateisystem, Netzwerk und andere Datenzugriffsfunktionen). Die meisten dieser APIs sind [für UWP-Apps verfügbar](https://msdn.microsoft.com/library/windows/apps/br211369). Sie können daher davon ausgehen, dass sich ein Großteil dieses Codes ohne Änderungen portieren lässt. |
| [Portieren für Formfaktor und Benutzerfreundlichkeit](wpsl-to-uwp-form-factors-and-ux.md) | Windows-Apps weisen auf PCs, Mobilgeräten und vielen anderen Arten von Geräten ein einheitliches Erscheinungsbild auf. Die Benutzeroberfläche, Eingabe und Interaktionsmuster sind fast identisch, und Benutzer profitieren von einer einheitlichen Umgebung auf allen Geräten.|
|[Fallstudie: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | In diesem Thema wird eine Fallstudie für das Portieren einer sehr einfachen Windows Phone Silverlight-App zu einer UWP-App für Windows 10 vorgestellt. Mit Windows 10 können Sie ein einzelnes App-Paket erstellen, das Ihre Kunden auf einer Vielzahl von Geräten installieren können – und genau das werden wir in dieser Fallstudie tun. |
| [Fallstudie: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | Diese Fallstudie baut auf den Informationen aus [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) auf und beginnt mit einer Windows Phone Silverlight-App, die gruppierte Daten in einem **LongListSelector**-Element anzeigt. Im Ansichtsmodell stellt jede Instanz der **Author**-Klasse die Gruppe der vom betreffenden Autor verfassten Titel dar. In **LongList Selector** können wir dann entweder die Bücherliste nach Autoren gruppiert anzeigen oder die Liste verkleinern, um eine Sprungliste der Autoren zu erhalten. |

## Verwandte Themen

**Dokumentation**
* [Neuigkeiten für Entwickler in Windows 10](https://dev.windows.com/getstarted/whats-new-windows-10)
* [Anleitung für Universelle Windows-Plattform-Apps (UWP)](https://msdn.microsoft.com/library/windows/apps/dn894631)
* [Roadmap für Universal Windows Platform (UWP)-Apps mit C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/br229583)
* [Nächste Schritte für Windows Phone 8-Entwickler](https://msdn.microsoft.com/library/windows/apps/xaml/dn655121.aspx)
**Magazine-Artikel**
* [Visual Studio Magazine: Windows Phone 8.1: Ein Riesenschritt für die Konvergenz](http://go.microsoft.com/fwlink/p/?LinkID=398541)
**Präsentationen**
* [The Story of Bringing Nokia Music from Windows Phone to Windows 8 (Beschreibung der Bereitstellung von Nokia Music von Windows Phone unter Windows 8)](http://go.microsoft.com/fwlink/p/?LinkId=321521)
 



<!--HONumber=Mar16_HO1-->


