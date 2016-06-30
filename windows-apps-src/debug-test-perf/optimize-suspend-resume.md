---
author: mcleblanc
ms.assetid: E1943DCE-833F-48AE-8402-CD48765B24FC
title: Optimieren von Anhalten/Fortsetzen
description: "Erstellen Sie Apps für die universelle Windows-Plattform (UWP), die die Verwendung des Prozesslebensdauer-Systems optimieren und nach dem Anhalten oder Beenden effizient fortgesetzt werden."
translationtype: Human Translation
ms.sourcegitcommit: e0f04c4242891b25db460d4852ab8cc070d82260
ms.openlocfilehash: 9fee4ab9c55c1243c04c2ed5f007412751528037

---
# Optimieren von Anhalten/Fortsetzen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Erstellen Sie Apps für die Universelle Windows-Plattform (UWP), die die Verwendung des Prozesslebensdauer-Systems optimieren und nach dem Anhalten oder Beenden effizient fortgesetzt werden.

## Starten

Überprüfen Sie beim Reaktivieren einer angehaltenen/beendeten App, ob das Reaktivieren lange gedauert hat. Ist dies der Fall, empfiehlt es sich unter Umständen, anstelle der veralteten Daten des Benutzers wieder die Hauptseite der App anzuzeigen. Dies beschleunigt auch den Start.

Überprüfen Sie während der Aktivierung immer „PreviousExecutionState“ des EventArgs-Parameters (bei gestarteten Aktivierungen etwa „LaunchActivatedEventArgs.PreviousExecutionState“). Vergeuden Sie bei „ClosedByUser“ oder „NotRunning“ keine Zeit mit der Wiederherstellung des zuvor gespeicherten Zustands. Stellen Sie in diesem Fall besser eine frische Benutzeroberfläche bereit, die außerdem schneller geladen wird.

Bei der Wiederherstellung des zuvor gespeicherten Zustands empfiehlt es sich unter Umständen, den Zustand nachzuverfolgen und nur bei Bedarf wiederherzustellen. Ein Beispiel: Angenommen, Ihre App wurde angehalten, hat den Zustand dreier Seiten gespeichert und wurde dann beendet. Wenn dem Benutzer beim nächsten Start die dritte Seite angezeigt werden soll, verzichten Sie auf die Wiederherstellung des Zustands der ersten beiden Seiten. Speichern Sie stattdessen diesen Zustand, und verwenden Sie ihn nur bei Bedarf.

## Während der Ausführung

Es empfiehlt sich nicht, auf das Anhalteereignis zu warten und dann umfangreiche Zustandsdaten zu speichern. Stattdessen sollte die Anwendung während der Ausführung schrittweise kleinere Mengen an Zustandsdaten speichern. Dies gilt insbesondere für umfangreiche Apps, denen beim Anhalten unter Umständen nicht genügend Zeit bleibt, um alles auf einmal zu speichern.

Sie müssen allerdings einen guten Kompromiss zwischen inkrementeller Speicherung und Leistung der ausgeführten App finden. Ein gutes Modell wäre, geänderte (und damit zu speichernde) Daten inkrementell nachzuverfolgen und das Anhalteereignis als Signal für die tatsächliche Speicherung der Daten zu nutzen. Das geht schneller als alle Daten zu speichern oder den gesamten Zustand der App zu prüfen, um zu entscheiden, was gespeichert werden muss.

Ermitteln Sie den Zeitpunkt für die Zustandsspeicherung nicht auf der Grundlage des Fensterereignisses „Activated“ oder „VisibilityChanged“. Wenn der Benutzer Ihre App verlässt, wird das Fenster zwar deaktiviert, die App wird jedoch erst etwas später (etwa nach zehn Sekunden) angehalten. Dies dient zur Verbesserung der Reaktionszeit für den Fall, dass der Benutzer schnell wieder zu Ihrer App zurückkehrt. Warten Sie mit dem Ausführen der Anhaltelogik auf das Eintreten des Anhalteereignisses.

## Anhalten

Verringern Sie den Speicherbedarf Ihrer angehaltenen App. Wenn Ihre angehaltene App weniger Arbeitsspeicher beansprucht, reagiert das gesamte System schneller, und es müssen weniger angehaltene Apps (einschließlich Ihrer eigenen) beendet werden. Achten Sie dabei jedoch darauf, den Speicherbedarf nicht so weit zu verringern, dass sich die Fortsetzung der App erheblich verlangsamt, weil Ihre App erst wieder große Datenmengen in den Arbeitsspeicher laden muss.

Bei verwalteten Apps wird nach Abschluss der Suspend-Handler der App ein Garbage Collection-Durchlauf ausgeführt. Machen Sie sich das zunutze, indem Sie Verweise auf Objekte freigeben, die zur Verringerung des Speicherbedarfs der angehaltenen App beitragen.

Im Idealfall ist die Anhaltelogik Ihrer App in weniger als einer Sekunde abgeschlossen. Je kürzer der Anhaltevorgang, desto besser. Dies kommt der Reaktionsgeschwindigkeit von anderen Apps und Teilen des Systems zugute. Bei Bedarf kann die Anhaltelogik bis zu fünf Sekunden (Desktopgeräte) bzw. bis zu zehn Sekunden (mobile Geräte) dauern. Nach diesem Zeitlimit wird die App abrupt beendet. Das gilt es zu vermeiden. Denn wenn der Benutzer wieder zu Ihrer App wechselt, wird ein neuer Prozess gestartet, was im Vergleich zur Fortsetzung einer angehaltenen App einen deutlich langsameren Eindruck hinterlässt.

## Fortsetzen

Da die meisten Apps beim Fortsetzen keinerlei besondere Aktion durchführen müssen, wird dieses Ereignis in der Regel nicht behandelt. Einige Apps stellen beim Fortsetzen Verbindungen wieder her, die beim Anhalten getrennt wurden, oder aktualisieren inzwischen veraltete Daten. Hierbei empfiehlt es sich, diese Aktivitäten nicht blindlings auszuführen, sondern die App so zu gestalten, dass sie nur bei Bedarf initiiert werden. Dadurch kann der Benutzer schneller zu einer angehaltenen App zurückwechseln, und es werden nur die wirklich erforderlichen Aufgaben ausgeführt.

## Vermeiden unnötiger Beendigungen

Das Prozesslebensdauer-System der universellen Windows-Plattform kann eine App aus den unterschiedlichsten Gründen anhalten oder beenden. Dieser Prozess ist so konzipiert, dass eine App schnell wieder in den Zustand versetzt werden kann, in dem sie sich befand, bevor sie angehalten oder beendet wurde. Im günstigsten Fall bemerkt der Benutzer nicht einmal, dass die App zwischenzeitlich nicht ausgeführt wurde. Hier folgen ein paar Tricks, mit denen Ihre UWP-App das System unterstützen kann, die Übergänge während der Lebensdauer einer App zu optimieren.

Eine App kann angehalten werden, wenn sie vom Benutzer in den Hintergrund versetzt wird oder das System in einen Energiesparmodus wechselt. Beim Anhalten löst die App das Anhalteereignis aus. Dabei hat sie maximal fünf Sekunden Zeit, um ihre Daten zu speichern. Wenn der Handler für das Anhalteereignis der App nicht innerhalb von fünf Sekunden abgeschlossen wird, geht das System davon aus, dass die App nicht mehr reagiert, und beendet sie. Eine beendete App muss erneut den langen Startprozess durchlaufen und kann nicht einfach direkt in den Speicher geladen werden, wenn der Benutzer sie wieder aufruft.

### Serialisierung nur wenn erforderlich

Viele Apps serialisieren sämtliche Daten, wenn Sie angehalten werden. Wenn Sie nur einen kleinen Teil der Einstellungsdaten der App speichern möchten, sollten Sie den [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622)-Speicher verwenden, anstatt die Daten zu serialisieren. Serialisieren Sie nur große Datenmengen, die nicht zu den Einstellungen gehören.

Wenn Sie Ihre Daten serialisieren, sollten Sie ein erneutes Serialisieren vermeiden, wenn sich die Daten nicht geändert haben. Dabei dauert es nicht nur länger, wenn die Daten serialisiert und gespeichert werden, sondern auch, wenn die Daten bei der Reaktivierung der App gelesen und deserialisiert werden. Stattdessen sollte die App feststellen, ob sich ihr Status tatsächlich geändert hat und ggf. nur die geänderten Daten serialisieren bzw. deserialisieren. Eine empfehlenswerte Methode hierfür ist die regelmäßige Serialisierung im Hintergrund von Daten, wenn sich diese ändern. Bei dieser Methode ist bereits alles gespeichert, was beim Anhalten serialisiert werden muss, und die App kann ohne große Verzögerung angehalten werden.

### Serialisieren von Daten in C# und Visual Basic

Als Serialisierungstechnologien für .NET-Apps stehen die Klassen [**System.Xml.Serialization.XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx), [**System.Runtime.Serialization.DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) und [**System.Runtime.Serialization.Json.DataContractJsonSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.json.datacontractjsonserializer.aspx) zur Verfügung.

Aus Leistungsgründen empfehlen wir die Verwendung der [**XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx)-Klasse. Die **XmlSerializer**-Klasse zeichnet sich durch die schnellste Serialisierung und Deserialisierung sowie durch einen geringen Speicherbedarf aus. **XmlSerializer** ist nur in wenigen Bereichen mit .NET Framework verknüpft. Im Vergleich zu den anderen Serialisierungstechnologien müssen daher für die Verwendung von **XmlSerializer** weniger Module in Ihre App geladen werden.

[
              **DataContractSerializer**
            ](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) vereinfacht die Serialisierung angepasster Klassen, auch wenn es größere Auswirkungen auf die Leistung als **XmlSerializer** hat. Wenn Sie eine bessere Leistung benötigen, denken Sie über einen Wechsel nach. Generell sollten Sie nur ein Serialisierungsprogramm laden und **XmlSerializer** vorziehen, falls Sie nicht die Funktionen eines anderen Serialisierungsprogramms benötigen.

### Verringern des Speicherbedarfs

Das System versucht, möglichst viele angehaltene Apps im Arbeitsspeicher vorzuhalten, damit der Benutzer schnell und zuverlässig zwischen den Apps wechseln kann. Solange sich eine angehaltene App im Arbeitsspeicher des Systems befindet, kann sie für Benutzerinteraktionen schnell wieder in den Vordergrund geholt werden, ohne dass ein Begrüßungsbildschirm angezeigt wird oder langwierige Ladevorgänge ausgeführt werden müssen. Sollten jedoch nicht genügend Ressourcen verfügbar sein, um eine App im Speicher zu belassen, wird die App beendet. Die Speicherverwaltung ist also aus zwei Gründen von Bedeutung:

-   Je mehr Speicher beim Anhalten freigegeben wird, desto geringer ist die Wahrscheinlichkeit, dass Ihre angehaltene App aufgrund unzureichender Ressourcen beendet wird.
-   Durch Verringern des Gesamtspeicherbedarfs Ihrer App verringern Sie außerdem die Wahrscheinlichkeit, dass andere angehaltene Apps beendet werden.

### Freigeben von Ressourcen

Bestimmte Objekte, beispielsweise Dateien und Geräte, beanspruchen Arbeitsspeicher in großem Umfang. Wir empfehlen, dass eine angehaltene App Handles für diese Objekte freigibt und sie erst bei Bedarf neu erstellt. Dies ist auch eine gute Gelegenheit zum Löschen sämtlicher Zwischenspeicherungen, die bei Reaktivierung der App nicht mehr gültig sind. Ein weiterer Schritt, den das XAML-Framework für Apps mit C# und Visual Basic gegebenenfalls ausführt, ist die Garbage Collection. Damit wird die Freigabe aller Objekte erreicht, auf die im App-Code nicht mehr verwiesen wird.

## Schnelles Reaktivieren

Die Ausführung einer angehaltenen App kann fortgesetzt werden, wenn sie vom Benutzer in den Vordergrund geholt wird oder wenn das System einen Energiesparmodus verlässt. Wenn eine angehaltene App fortgesetzt wird, wird die App-Ausführung an dem Punkt fortgesetzt, an dem sie zuvor angehalten wurde. Es gehen keine App-Daten verloren, da sie im Arbeitsspeicher gespeichert wurden. Dies gilt auch dann, wenn die App über einen längeren Zeitraum angehalten war.

Bei den meisten Apps ist keine Behandlung des [**Resuming**](https://msdn.microsoft.com/library/windows/apps/BR205859)-Ereignisses erforderlich. Bei der App-Reaktivierung besitzen die Variablen und Objekte exakt den gleichen Zustand wie beim Anhalten der App. Behandeln Sie das **Resuming**-Ereignis nur dann, wenn Sie Daten oder Objekte aktualisieren müssen, die sich nach dem Anhalten der App möglicherweise geändert haben. Beispiele wären etwa Inhalte (z. B. aktualisierte Feeddaten), nicht mehr aktuelle Netzwerkverbindungen oder die Wiederherstellung des Zugriffs ein Gerät (etwa eine Webcam).

## Verwandte Themen

* [Richtlinien für das Anhalten und Fortsetzen von Apps](https://msdn.microsoft.com/library/windows/apps/Hh465088)
 

 







<!--HONumber=Jun16_HO4-->


