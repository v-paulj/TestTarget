---
author: mcleblanc
ms.assetid: 64F7FC51-E8AC-4098-9C5F-0172E4724B5C
title: Leistung
description: "Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: f4589b187d05ae122839fab74d8086779847b120

---
# Leistung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen. Technisch gesehen ist die Leistung keine funktionale Anforderung. Wenn Sie die Leistung aber als Feature behandeln, hilft es Ihnen dabei, die Erwartungen der Benutzer zu erfüllen. Das Festlegen von Zielen und deren Messung sind wichtige Faktoren. Ermitteln Sie die für Sie leistungskritischen Szenarien, und legen Sie fest, was unter guter Leistung zu verstehen ist. Messen Sie die Ziele dann während des gesamten Lebenszyklus Ihres Projekts frühzeitig und häufig, um sicherzustellen, dass Sie Ihre Ziele erreichen. In diesem Abschnitt erfahren Sie, wie Sie Ihren Leistungsworkflow strukturieren, Animationsfehler und Probleme mit der Bildfrequenz beheben und Startzeit, Seitennavigationszeit und Speicherverwendung optimieren.

Durch die Portierung Ihrer App auf Windows 10 lassen sich erhebliche Leistungssteigerungen erzielen (sofern noch nicht geschehen). Verschiedene XAML-Optimierungen (z. B. [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)) sind nur in Windows 10-Apps verfügbar. Weitere Informationen finden Sie unter [Portieren von Apps auf Windows 10](https://msdn.microsoft.com/library/windows/apps/Mt238321) und in der Sitzung „//build/“ unter [Wechsel zur universellen Windows-Plattform](http://channel9.msdn.com/Events/Build/2015/3-741).

| Thema | Beschreibung |
|-------|-------------|
| [Planen der Leistung](planning-and-measuring-performance.md) | Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen. Technisch gesehen ist die Leistung keine funktionale Anforderung. Wenn Sie die Leistung aber als Feature behandeln, hilft es Ihnen dabei, die Erwartungen der Benutzer zu erfüllen. Das Festlegen von Zielen und deren Messung sind wichtige Faktoren. Ermitteln Sie die für Sie leistungskritischen Szenarien, und legen Sie fest, was unter guter Leistung zu verstehen ist. Messen Sie die Ziele dann während des gesamten Lebenszyklus Ihres Projekts frühzeitig und häufig, um sicherzustellen, dass Sie Ihre Ziele erreichen. |
| [Optimieren der ListView- und GridView-Benutzeroberfläche](optimize-gridview-and-listview.md) | Verbessern Sie die Leistung und Startzeit von [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) durch UI-Virtualisierung, Elementreduzierung und progressive Aktualisierung von Elementen. |
| [Virtualisierung von ListView- und GridView-Daten](listview-and-gridview-data-optimization.md) | Verbessern Sie die Leistung und Startzeit von [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) durch Datenvirtualisierung. |
| [Verbessern der Leistung bei der Garbage Collection](improve-garbage-collection-performance.md) | In C# und Visual Basic geschriebene UWP-Apps profitieren von der automatischen Arbeitsspeicherverwaltung des .NET Garbage Collectors. Dieser Abschnitt bietet einen Überblick über das Verhalten des .NET Garbage Collectors sowie über die bewährten Methoden zur Leistungssteigerung für den Garbage Collector in UWP-Apps. |
| [Aufrechterhalten der Reaktionsfähigkeit des UI-Threads](keep-the-ui-thread-responsive.md) | Benutzer erwarten, dass eine App beim Durchführen einer Berechnung reaktionsfähig bleibt, unabhängig vom jeweiligen Computertyp. Das bedeutet für jede App etwas anderes. Für einige Apps bedeutet dies z.B. Folgendes: realistischere physische Effekte bereitstellen, Daten vom Datenträger oder aus dem Internet schneller laden, komplexe Szenen schnell darstellen, schnell zwischen Seiten navigieren, Anweisungen im Nu finden oder schnelles Verarbeiten von Daten. Unabhängig von der Art der Berechnung möchten Benutzer, dass die App auf ihre Eingabe reagiert. Es stört sie, wenn die App scheinbar nicht reagiert, während sie &quot;denkt&quot;. |
| [Optimieren Ihres XAML-Markups](optimize-xaml-loading.md) | Die Analyse von XAML-Markup zum Erstellen von Objekten im Arbeitsspeicher kann für eine komplexe Benutzeroberfläche viel Zeit in Anspruch nehmen. Hier finden Sie einige Punkte, die Sie zur Optimierung der XAML-Markupanalyse, Ladezeit und Effizienz des Arbeitsspeichers für Ihre App vornehmen können. | 
| [Optimieren des XAML-Layouts](optimize-your-xaml-layout.md) | Das Layout kann sowohl bezüglich der CPU-Auslastung als auch des Aufwands ein ressourcenintensiver Teil einer XAML-App sein. Hier sind einige einfache Schritte, mit denen Sie die Layoutleistung Ihrer XAML-App verbessern können. | 
| [Tipps zu MVVM und Sprachleistung](mvvm-performance-tips.md) | In diesem Thema werden einige Leistungsaspekte in Bezug auf die Wahl von Softwaredesignmustern und Programmiersprachen erläutert. |
| [Bewährte Methoden für die Leistung Ihrer App beim Starten](best-practices-for-your-app-s-startup-performance.md) | Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit optimalen Startzeiten, indem Sie die Vorgehensweise beim Starten und Aktivieren optimieren. |
| [Optimieren von Animationen, Medien und Bildern](optimize-animations-and-media.md) | Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit flüssigen Animationen, hoher Bildfrequenz und leistungsstarker Medienaufzeichnung und -wiedergabe. |
| [Optimieren von Anhalten/Fortsetzen](optimize-suspend-resume.md) | Erstellen Sie UWP-Apps Universelle Windows-Plattform), die die Verwendung des Prozesslebensdauer-Systems optimieren und nach dem Anhalten oder Beenden effizient fortgesetzt werden. |
| [Optimieren des Dateizugriffs](optimize-file-access.md) | Erstellen Sie UWP-Apps, die effizient auf das Dateisystem zugreifen und dadurch Leistungsprobleme aufgrund von Datenträgerlatenz und Arbeitsspeicher-/CPU-Zyklen vermeiden. |
| [Komponenten für Windows-Runtime und Optimieren der Interoperabilität](windows-runtime-components-and-optimizing-interop.md) | Erstellen Sie UWP-Apps, die UWP-Komponenten verwenden, mit systemeigenen und verwalteten Typen zusammenarbeiten und gleichzeitig Probleme mit der Interoperabilitätsleistung vermeiden. |
| [Tools für Profilerstellung und Leistung](tools-for-profiling-and-performance.md) | Microsoft bietet verschiedene Tools zur Verbesserung der Leistung Ihrer UWP-App.|




<!--HONumber=Aug16_HO3-->


