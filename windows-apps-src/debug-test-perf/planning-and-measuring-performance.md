---
author: mcleblanc
ms.assetid: A37ADD4A-2187-4767-9C7D-EDE8A90AA215
title: Planen der Leistung
description: "Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen."
translationtype: Human Translation
ms.sourcegitcommit: afb508fcbc2d4ab75188a2d4f705ea0bee385ed6
ms.openlocfilehash: a53434223585d2c36fe30f4c2a49f019c7552662

---
# Planen der Leistung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen. Technisch gesehen ist die Leistung keine funktionale Anforderung. Wenn Sie die Leistung aber als Feature behandeln, hilft es Ihnen dabei, die Erwartungen der Benutzer zu erfüllen. Das Festlegen von Zielen und deren Messung sind wichtige Faktoren. Ermitteln Sie die für Sie leistungskritischen Szenarien, und legen Sie fest, was unter guter Leistung zu verstehen ist. Messen Sie die Ziele dann während des gesamten Lebenszyklus Ihres Projekts frühzeitig und häufig, um sicherzustellen, dass Sie Ihre Ziele erreichen.

## Festlegen der Ziele

Eine einfache Methode zum Definieren einer guten Leistung ist die Benutzererfahrung. Die Startzeit einer App kann sich darauf auswirken, wie ein Benutzer die Leistung der App wahrnimmt. Eine App-Startzeit von unter einerSekunde kann als hervorragende Leistung erachtet werden, weniger als 5Sekunden als gute Leistung und länger als 5Sekunden als schwache Leistung.

Andere Metriken haben weniger offensichtliche Auswirkungen auf die Benutzerfreundlichkeit, z.B. der Arbeitsspeicher. Die Wahrscheinlichkeit, dass eine App beendet wird, während sie entweder angehalten ist oder nicht mehr reagiert, steigt mit zunehmendem Speicherbedarf der aktiven App. Eine hohe Speicherauslastung verringert im Allgemeinen die Benutzerfreundlichkeit für alle Apps auf dem System, daher ist das Ziel einer angemessenen Speicherauslastung begründet. Berücksichtigen Sie die vom Benutzer wahrgenommene ungefähre Größe der App: klein, mittel oder groß. Erwartungen in Bezug auf die Leistung korrelieren mit dieser Einschätzung. Beispiel: Sie möchten eine kleine App entwickeln, die nicht viele Medien nutzt und weniger als 100MB Arbeitsspeicher belegt.

Es ist besser, ein erstes Ziel festzulegen und es später zu überdenken, anstatt überhaupt kein Ziel zu haben. Die Leistungsziele für Ihre App sollten genau bezeichnet und messbar sein sowie in drei Kategorien fallen: Wie lange dauert es für Benutzer oder die App, Aufgaben auszuführen (Zeit)? Mit welcher Frequenz und Kontinuität stellt sich die App selbst als Reaktion auf Benutzerinteraktionen neu dar (Flüssigkeit)? Wie gut spart die App Systemressourcen einschließlich der Akkuleistung (Effizienz)?

## Zeit

Überlegen Sie sich die zulässigen Bereiche für die verstrichene Zeit (*Interaktionsklassen*), die für Benutzer erforderlich ist, um ihre Aufgaben in der App zu erledigen. Weisen Sie jeder Interaktionsklasse eine Bezeichnung, eine Benutzerwahrnehmung sowie eine ideale und eine maximale Dauer zu. Hier sind einige Empfehlungen.

| Bezeichnung der Interaktionsklasse | Benutzerwahrnehmung                 | Ideal            | Maximal          | Beispiele                                                                     |
|-------------------------|---------------------------------|------------------|------------------|------------------------------------------------------------------------------|
| Schnell                    | Minimal erkennbare Verzögerung      | 100Millisekunden | 200Millisekunden | App-Leiste aufrufen; Schaltfläche betätigen (erste Reaktion)                        |
| Standard                 | Zügig, aber nicht schnell             | 300Millisekunden | 500Millisekunden | Größe ändern; semantischer Zoom                                                        |
| Dynamisch              | Nicht zügig, aber dynamisch | 500Millisekunden | 1Sekunde         | Zu einer anderen Seite navigieren; angehaltene App fortsetzen          |
| Start                  | Nicht flüssig          | 1Sekunde         | 3Sekunden        | App zum ersten Mal starten oder App aufrufen, nachdem sie zuvor beendet wurde |
| Kontinuierlich              | Wird nicht mehr als dynamisch wahrgenommen      | 500Millisekunden | 5Sekunden        | Datei aus dem Internet herunterladen                                            |
| Träge                 | Lang; Benutzer könnte Interesse verlieren    | 500Millisekunden | 10Sekunden       | Mehrere Apps aus dem Store installieren                                         |

 

Sie können den Leistungsszenarien Ihrer App jetzt Interaktionsklassen zuweisen. Sie können jedem Szenario den Bezugszeitpunkt der App, einen Teil der Benutzererfahrung und eine Interaktionsklasse zuweisen. Hier folgen einige Vorschläge für eine Beispiel-App für Lebensmittel und Gastronomie.


<!-- DHALE: used HTML table here b/c WDCML src used rowspans -->
<table>
<tr><th>Szenario</th><th>Zeitpunkt</th><th>Benutzererfahrung</th><th>Interaktionsklasse</th></tr>
<tr><td rowspan="3">Zur Rezeptseite navigieren </td><td>Erste Reaktion</td><td>Seitenübergangsanimation gestartet</td><td>Schnell (100 - 200Millisekunden)</td></tr>
<tr><td>Dynamisch</td><td>Zutatenliste gestartet; keine Bilder</td><td>Dynamisch (500Millisekunden bis 1Sekunde)</td></tr>
<tr><td>Alle Elemente sichtbar</td><td>Gesamter Inhalt geladen; Bilder angezeigt</td><td>Kontinuierlich (500Millisekunden - 5Sekunden)</td></tr>
<tr><td rowspan="2">Rezeptsuche</td><td>Erste Reaktion</td><td>Klick auf Suchschaltfläche</td><td>Schnell (100 - 200Millisekunden)</td></tr>
<tr><td>Alle Elemente sichtbar</td><td>Liste der lokalen Rezepte angezeigt</td><td>Standard (300 - 500Millisekunden)</td></tr>
</table>

Wenn Sie Live-Inhalte anzeigen, berücksichtigen Sie auch die Ziele hinsichtlich der Aktualität des Inhalts. Ist als Ziel vorgegeben, den Inhalt alle paar Sekunden zu aktualisieren? Oder ist es für die Benutzererfahrung akzeptabel, den Inhalt alle paar Minuten, Stunden oder sogar nur einmal täglich zu aktualisieren?

Mit den angegebenen Zielen können Sie Ihre App jetzt besser testen, analysieren und optimieren.

## Flüssigkeit

Zu den bestimmten messbaren Zielen hinsichtlich der Flüssigkeit für Ihre App gehören:

-   Keine Stopps und Starts (Störungen) beim erneuten Rendern des Bildschirms.
-   Animationen werden mit 60 Bildern pro Sekunde (BpS) gerendert.
-   Wenn ein Benutzer eine Verschiebung bzw. einen Bildlauf ausführt, stellt die App 3 bis 6 Inhaltsseiten pro Sekunde dar.

## Effizienz

Zu den bestimmten messbaren Zielen hinsichtlich der Effizienz für Ihre App gehören:

-   Für den Prozess Ihrer App liegt der CPU-Prozentsatz bei oder unter *N*, und die Speicherauslastung in MB liegt jederzeit bei oder unter *M*.
-   Wenn die App inaktiv ist, sind *N* und *M* 0 für den Prozess Ihrer App.
-   Ihre App kann bei Akkubetrieb bis zu *X* Stunden aktiv genutzt werden. Wenn Ihre App inaktiv ist, behält das Gerät seine Ladung für *Y* Stunden.

## Leistungsorientiertes Entwerfen der App

Sie können jetzt die Leistungsziele verwenden, um den Entwurf Ihrer App zu beeinflussen. Nachdem der Benutzer bei der Verwendung der Beispiel-App für Lebensmittel und Gastronomie zur Rezeptseite navigiert ist, können Sie festlegen, dass [Elemente inkrementell aktualisiert](optimize-gridview-and-listview.md#update-items-incrementally) werden, damit der Name des Rezepts zuerst gerendert, die Anzeige der Zutaten zurückgestellt und die Anzeige der Abbildungen noch weiter zurückgestellt wird. Dadurch bleiben die Reaktionsfähigkeit und eine flüssige Benutzeroberfläche bei der Verschiebung bzw. beim Bildlauf erhalten, wobei das Rendering mit höchster Genauigkeit dann stattfindet, nachdem sich die Interaktion auf ein Tempo verlangsamt hat, bei dem der UI-Thread mithalten kann. Im Folgenden sind einige Aspekte aufgeführt, die auch berücksichtigt werden sollten.

**UI**

-   Optimieren Sie die zum Analysieren und Laden erforderliche Zeit sowie die Effizienz für jede Seite der Benutzeroberfläche Ihrer App (insbesondere die Ausgangsseite), indem Sie das [XAML-Markup optimieren](optimize-xaml-loading.md). Stellen Sie kurz gesagt das Laden der Benutzeroberfläche und des Codes zurück, bis dies erforderlich ist.
-   Weisen Sie allen [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)- und [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)-Elementen dieselbe Größe zu, und verwenden Sie möglichst viele [ListView- und GridView-Optimierungsverfahren](optimize-gridview-and-listview.md).
-   Deklarieren Sie die Benutzeroberfläche in Form von Markup, das vom Framework geladen und in Blöcken wiederverwendet werden kann, anstatt sie zwingend in Code zu erstellen.
-   Reduzieren Sie UI-Elemente, bis sie vom Benutzer benötigt werden. Weitere Informationen finden Sie unter der [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992)-Eigenschaft.
-   Bevorzugen Sie Designübergänge und -animationen vor Storyboardanimationen. Weitere Informationen finden Sie unter [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/Mt187350). Bedenken Sie, dass Storyboardanimationen kontinuierliche Aktualisierungen auf dem Bildschirm erfordern und die CPU und Grafikpipeline auslasten. Führen Sie keine Animationen aus, wenn der Benutzer nicht mit der App interagiert, um den Stromverbrauch zu verringern.
-   Von Ihnen geladene Bilder sollten dabei eine Größe aufweisen, die für die jeweilige Ansicht geeignet ist. Verwenden Sie deshalb die [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210)-Methode.

**CPU, Arbeitsspeicher und Leistung**

-   Planen Sie die Ausführung von Aufgaben mit niedriger Priorität für Threads und/oder Kerne mit niedrigerer Priorität. Weitere Informationen finden Sie unter [Asynchrone Programmierung](https://msdn.microsoft.com/library/windows/apps/Mt187335) und den Angaben zur [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR209054)-Eigenschaft bzw. [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211)-Klasse.
-   Minimieren Sie den Speicherbedarf Ihrer angehaltenen App durch Freigeben speicherintensiver Ressourcen (z. B. Medien).
-   Minimieren Sie den Arbeitssatz des Codes.
-   Vermeiden Sie Arbeitsspeicherverluste, indem Sie die Registrierung von Ereignishandlern aufheben und UI-Elemente wenn möglich dereferenzieren.
-   Seien Sie beim Abrufen von Daten, Sensoren oder beim Planen von Aufgaben für die CPU zurückhaltend, wenn diese im Leerlauf ist, um den Stromverbrauch zu reduzieren.

**Datenzugriff**

-   Rufen Sie Inhalte vorab ab, wenn dies möglich ist. Informationen zum automatischen Vorabrufen finden Sie in den Angaben zur [**ContentPrefetcher**](https://msdn.microsoft.com/library/windows/apps/Dn279042)-Klasse. Informationen zum manuellen Vorabrufen finden Sie in den Angaben zum [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/BR224847)-Namespace und zur [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Hh700517)-Klasse.
-   Speichern Sie Inhalte, für die der Zugriff aufwendig ist, nach Möglichkeit im Cache. Weitere Informationen finden Sie unter den Eigenschaften [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/BR241621) und [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622).
-   Zeigen Sie für Cachefehler so schnell wie möglich eine Platzhalter-UI an, die angibt, dass die App weiterhin Inhalte lädt. Führen Sie den Übergang von Platzhaltern zu Liveinhalten so durch, dass Benutzer sich nicht gestört fühlen. Ändern Sie z.B. nicht die Position der Inhalte unter dem Finger oder Mauszeiger des Benutzers, während von der App Liveinhalte geladen werden.

**Starten und Fortsetzen von Apps**

-   Verzögern Sie den Begrüßungsbildschirm der App, und erweitern Sie diesen Begrüßungsbildschirm nur, wenn dies notwendig ist. Weitere Informationen finden Sie unter [Schaffen einer schnellen und flüssigen App-Starterfahrung](http://go.microsoft.com/fwlink/p/?LinkId=317595) und [Längere Anzeige des Begrüßungsbildschirms](https://msdn.microsoft.com/library/windows/apps/Mt187309).
-   Deaktivieren Sie Animationen, die direkt nach dem Schließen des Begrüßungsbildschirms erscheinen, da dies lediglich als Verlängerung des Startzeitraums der App angesehen wird.

**Adaptive UI und Ausrichtung**

-   Verwenden Sie die [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021)-Klasse.
-   Führen Sie nur die erforderlichen Arbeitsschritte sofort durch, und verschieben Sie aufwendige App-Vorgänge auf einen späteren Zeitpunkt. Für die App stehen zur Durchführung der Arbeitsschritte zwischen 200 und 800 Millisekunden zur Verfügung, bevor Benutzer die UI der App angezeigt wird.

Sobald Ihre leistungsbezogenen Entwürfe fertig sind, können Sie mit der Codierung der App beginnen.

## Leistungsorientierte Instrumentation

Fügen Sie beim Codieren Code hinzu, der Meldungen und Ereignisse an bestimmten Punkten der App-Ausführung protokolliert. Wenn Sie Ihre App später testen, können Sie Profilerstellungstools wie Windows Performance Recorder und Windows Performance Analyzer (beide enthalten im [Windows Performance Toolkit](https://msdn.microsoft.com/library/windows/apps/xaml/hh162945.aspx)) verwenden, um einen Bericht zur App-Leistung zu erstellen und anzuzeigen. In diesem Bericht können Sie nach diesen Meldungen und Ereignissen Ausschau halten, um die Ergebnisse des Berichts besser analysieren zu können.

Die Universelle Windows-Plattform (UWP) stellt Protokollierungs-APIs bereit, die von der [Ereignisablaufverfolgung für Windows (Event Tracing for Windows, ETW)](https://msdn.microsoft.com/library/windows/desktop/Bb968803)unterstützt werden. Gemeinsam steht damit eine umfassende Lösung für Ereignisprotokollierung und Ablaufverfolgung bereit. Die APIs, die Teil des [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/BR206677)-Namespace sind, enthalten die Klassen [**FileLoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264138), [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195), [**LoggingChannel**](https://msdn.microsoft.com/library/windows/apps/Dn264202) und [**LoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264217).

Um zu einem bestimmten Zeitpunkt während der App-Ausführung eine Meldung im Bericht zu protokollieren, erstellen Sie ein **LoggingChannel**-Objekt. Rufen Sie dann die [**LogMessage**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingchannel.logmessage.aspx)-Methode des Objekts wie folgt auf.

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingChannel myLoggingChannel = new LoggingChannel("MyLoggingChannel");

myLoggingChannel.LogMessage(LoggingLevel.Information, "Here' s my logged message.");

// ...
```

Wenn Sie die Start- und Stoppereignisse während der App-Ausführung über einen Zeitraum hinweg im Bericht protokollieren möchten, erstellen Sie ein **LoggingActivity**-Objekt und rufen dann dessen [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingactivity.loggingactivity.aspx)-Konstruktor auf, wie hier zu sehen.

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingActivity myLoggingActivity;

// myLoggingChannel is defined and initialized in the previous code example.
using (myLoggingActivity = new LoggingActivity("MyLoggingActivity"), myLoggingChannel))
{   // After this logging activity starts, a start event is logged.
    
    // Add code here to do something of interest.
    
}   // After this logging activity ends, an end event is logged.

// ...
```

Weitere Informationen finden Sie im [Beispiel für die Protokollierung](http://go.microsoft.com/fwlink/p/?LinkId=529576).

Nachdem Sie Ihre App instrumentiert haben, können Sie die App-Leistung testen und messen.

## Testen und Messen mit Leistungszielen

Teil Ihres Leistungsplans ist das Definieren aller während der Entwicklung auftretenden Punkte, an denen Sie die Leistung messen möchten. Dies dient verschiedenen Zwecken, je nachdem, ob die Leistung bei der Prototyperstellung, Entwicklung oder Bereitstellung gemessen wird. Das Messen der Leistung in den frühen Phasen der Prototyperstellung kann überaus hilfreich sein, daher wird empfohlen, dass Sie dies tun, sobald Sie über Code verfügen, der entscheidende Aufgaben erledigt. Durch frühe Messungen erhalten Sie eine gute Vorstellung davon, wo der Leistungsaufwand in Ihrer App hoch ist. Basierend auf diesen Infos können Sie fundierte Designentscheidungen treffen. Sie können so Apps mit hoher Leistung entwickeln, die sich gut skalieren lassen. Es ist im Allgemeinen aufwendiger, das Design später als früher zu ändern. Wird die Leistung erst spät im Produktzyklus gemessen, kann dies Codeänderungen in letzter Minute sowie eine schlechte Leistung zur Folge haben.

Verwenden Sie diese Verfahren und Tools zum Testen, wie Ihre App gegenüber den ursprünglichen Leistungszielen dasteht.

-   Führen Sie den Test für eine Vielzahl von Hardwarekonfigurationen durch, einschließlich All-in-One- und Desktop-PCs, Laptops, Ultrabooks und Tablets sowie anderer mobiler Geräte.
-   Führen Sie die Tests für eine Vielzahl von Bildschirmgrößen durch. Bei breiteren Bildschirmen kann zwar mehr Inhalt angezeigt werden, die Darstellung dieses zusätzlichen Inhalts kann sich jedoch negativ auf die Leistung auswirken.
-   Schließen Sie so viele Testvariablen wie möglich aus.
    -   Deaktivieren Sie auf dem Testgerät die Hintergrund-Apps. Wählen Sie dazu in Windows aus dem Startmenü **Einstellungen** &gt; **Personalisierung** &gt; **Sperrbildschirm** aus. Wählen Sie jede aktive App aus, und wählen Sie dann **Keine**.
    -   Kompilieren Sie Ihre App in systemeigenen Code, indem Sie sie in der Releasekonfiguration erstellen, bevor sie auf dem Testgerät bereitgestellt wird.
    -   Um sicherzustellen, dass die automatische Wartung keinen Einfluss auf die Leistung des Testgeräts hat, lösen Sie es manuell aus und warten Sie, bis der Vorgang abgeschlossen ist. Suchen Sie in Windows im Startmenü nach **Sicherheit und Wartung**. Wählen Sie im Bereich **Wartung** unter **Automatische Wartung** die Option **Wartung starten** aus, und warten Sie, bis sich der Status **Wartung wird durchgeführt** ändert.
    -   Führen Sie die App mehrmals aus, um zufällige Testvariablen so gut es geht auszuschließen und dadurch konsistente Messungen zu ermöglichen.
-   Führen Sie Tests zur verringerten Leistungsverfügbarkeit durch. Das Gerät der Benutzer weist möglicherweise eine deutlich geringere Leistung als das Entwicklungssystem auf. Windows wurde unter Berücksichtigung von Geräten mit niedrigem Stromverbrauch, z.B. mobile Geräte, konzipiert. Apps, die auf der Plattform ausgeführt werden, sollten sicherstellen, dass sie auch auf diesen Geräten ordnungsgemäß ausgeführt werden können. Sie können davon ausgehen, dass ein energiesparendes Gerät ungefähr viermal langsamer ist als ein Desktop-PC. Legen Sie Ihre Ziele entsprechend fest.
-   Verwenden Sie eine Kombination von Tools wie Microsoft Visual Studio und Windows Performance Analyzer, um die App-Leistung zu messen. Visual Studio stellt eine Analyse bereit, die auf die App ausgerichtet ist, beispielsweise die Quellcodeverknüpfung. Windows Performance Analyzer bietet dagegen eine systemorientierte Analyse, beispielsweise die Bereitstellung von Systeminfos, Infos zu Touchmanipulationsereignissen sowie Infos zur Datenträger-E/A und Grafikprozessorauslastung (GPU). Beide Tools können Ablaufverfolgungsdateien sammeln und exportieren und freigegebene sowie Post-Mortem-Traces erneut öffnen.
-   Bevor Sie Ihre App zur Zertifizierung an den Store übermitteln, sollten Sie die leistungsbezogenen Testfälle in Ihre Testpläne integrieren, so wie im Abschnitt „Leistungstests“ unter [Tests im Zertifizierungskit für Windows-Apps](windows-app-certification-kit-tests.md) und im Abschnitt „Leistung und Stabilität“ unter [Testfälle für WindowsStore-Apps](https://msdn.microsoft.com/library/windows/apps/Dn275879) beschrieben.

Weitere Informationen finden Sie unter diesen Ressourcen und Tools zur Profilerstellung.

-   [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/apps/xaml/hh448170.aspx)
-   [Windows Performance Toolkit](https://msdn.microsoft.com/library/windows/apps/xaml/hh162945.aspx)
-   [Analysieren der Leistung mithilfe von Visual Studio-Diagnosetools](https://msdn.microsoft.com/library/windows/apps/xaml/hh696636.aspx)
-   Die „//build/“-Sitzung [XAML-Leistung](https://channel9.msdn.com/Events/Build/2015/3-698)
-   Die „//build/“-Sitzung [Neue XAML-Tools in Visual Studio2015](https://channel9.msdn.com/Events/Build/2015/2-697)

## Reagieren auf die Leistungstestergebnisse

Überprüfen Sie nach der Analyse der Leistungstestergebnisse, ob Änderungen erforderlich sind, beispielsweise:

-   Sollten Sie Entscheidungen hinsichtlich des App-Designs ändern oder den Code optimieren?
-   Ist es notwendig, die Instrumentation im Code zu entfernen oder zu ändern oder eine Instrumentation hinzufügen?
-   Sollen einige der Leistungsziele geändert werden?

Wenn Änderungen erforderlich sind, nehmen Sie diese vor, und kehren Sie dann zum Instrumentieren oder Testen zurück, und wiederholen Sie den Vorgang.

## Optimieren

Optimieren Sie nur die leistungskritischen Codepfade in Ihrer App. Dies sind die Codepfade, für die Sie die meiste Zeit aufgewendet haben. Die Profilerstellung wird Ihnen mitteilen, welche dies sind. Häufig ist ein Kompromiss nötig zwischen dem Erstellen einer Software, die auf guten Deisgnmethoden basiert, und dem Schreiben von Code, der die höchste Optimierung sicherstellt. In den Bereichen, in denen die Leistung keine wesentliche Rolle spielt, ist es im Allgemeinen besser, mehr Gewicht auf Entwicklerproduktivität und gutes Softwaredesign zu legen.




<!--HONumber=Aug16_HO3-->


