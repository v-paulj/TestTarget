---
author: mcleblanc
description: "Bei einer universellen 8.1-App – ob für Windows 8.1, Windows Phone 8.1 oder beides – werden Sie feststellen, dass der Quellcode und Ihre Kenntnisse sich reibungslos zu Windows 10 portieren lassen."
title: Wechsel von Windows-Runtime 8.x zu UWP
ms.assetid: ac163b57-dee0-43fa-bab9-8c37fbee3913
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3aa24e61482054dadd9b798063d46abf36623e9b

---

# Wechsel von Windows-Runtime 8.x zu UWP

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Bei einer universellen 8.1-App – ob für Windows 8.1, Windows Phone 8.1 oder beides – werden Sie feststellen, dass der Quellcode und Ihre Kenntnisse sich reibungslos zu Windows 10 portieren lassen. Mit Windows 10 können Sie eine UWP (Universelle Windows-Plattform)-App erstellen, also ein einzelnes App-Paket, das Ihre Kunden auf jeder Art von Gerät installieren können. Unter [Anleitung für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn894631)finden Sie weitere Hintergrundinformationen zu Windows 10, UWP-Apps und den Konzepten von adaptivem Code und adaptiver UI, die wir in diesem Portierungshandbuch erörtern werden.

Beim Portieren werden Sie feststellen, dass sich Windows 10 und die bisherigen Plattformen den Großteil aller APIs sowie das XAML-Markup, Benutzeroberflächenframework und die meisten Werkzeuge teilen, sodass Ihnen die Umgebung sehr vertraut sein sollte. Genau wie vorher, können Sie weiterhin zwischen C++, C# und Visual Basic als Programmiersprache mit dem XAML-Benutzeroberflächenframework wählen. Die ersten Schritte bei der Planung, was genau mit der aktuellen App oder den Apps geschehen soll, hängt von der Art der vorhandenen Apps und Projekte ab. Dies wird in den folgenden Abschnitten erläutert.

## Bei einer universellen 8.1-App

Eine universelle 8.1-App wird aus einem universellen 8.1-App-Projekt erstellt. Angenommen, der Name des Projekts ist „AppName\_81“. Es enthält diese untergeordneten Projekte.

-   AppName\_81.Windows. Dies ist das Projekt, das das App-Paket für Windows 8.1 erstellt.
-   AppName\_81.WindowsPhone. Dies ist das Projekt, das das App-Paket für Windows Phone 8.1 erstellt.
-   AppName\_81.Shared. Dies ist das Projekt, das den Quellcode, die Markupdateien und andere Assets und Ressourcen enthält, die von den beiden anderen Projekten verwendet werden.

Häufig bietet eine universelle Windows-App für 8.1 die gleichen Features (und das anhand des gleichen Codes und Markups) sowohl in Windows 8.1- als auch den Windows Phone 8.1-Formularen. Eine solche App ist ideal für die Portierung zu einer einzelnen Windows 10-App, die auf die universelle Gerätefamilie ausgerichtet ist (und die Sie auf der breitest möglichen Palette von Geräten installieren können). Sie portieren im Grunde den Inhalt des freigegebenen Projekts und müssen nur wenige oder gar keine Elemente von den anderen beiden Projekten verwenden, da sie nur wenig oder keinen Inhalt haben.

In anderen Fällen enthalten die Windows 8.1- und/oder Windows Phone 8.1-Formulare der App einzigartige Features. Oder sie enthalten den gleichen Featureumfang, implementieren diese Features jedoch mit verschiedenen Techniken oder anderen Technologien. Mit einer solchen App haben Sie die Möglichkeit, sie zu einer einzelnen App zu portieren, die auf die universelle Gerätefamilie ausgerichtet ist (in diesem Fall sollte sich die App selbst an verschiedene Geräte anpassen), oder Sie können sie zu mehr als einer App portieren, z. B. zu einer, die auf die Desktopgerätefamilie ausgerichtet ist, und einer für die Mobilgerätefamilie. Die Art der universellen 8.1-App bestimmt, welche Option für diesen Fall am besten geeignet ist.

1.  Portieren Sie den Inhalt des freigegebenen Projekts zu einer App für die universelle Gerätefamilie. Übernehmen Sie ggf. andere Inhalte aus den Windows- und WindowsPhone-Projekten, und verwenden Sie diese Inhalte bedingungslos in der App oder bedingt auf dem Gerät, auf dem Ihre App zu diesem Zeitpunkt ausgeführt wird (das letztere Verhalten wird auch als *adaptiv* bezeichnet).
2.  Portieren Sie den Inhalt des WindowsPhone-Projekts zu einer App für universelle Geräte. Retten Sie ggf. andere Inhalte aus dem Windows-Projekt, und verwenden Sie sie bedingungslos oder adaptiv.
3.  Portieren Sie den Inhalt des Windows-Projekts zu einer App für universelle Geräte. Retten Sie ggf. andere Inhalte aus dem WindowsPhone-Projekt, und verwenden Sie sie bedingungslos oder adaptiv.
4.  Portieren Sie den Inhalt des Windows-Projekts in eine App für universelle Geräte oder Desktopgeräte, und portieren Sie auch den Inhalt des WindowsPhone-Projekts zu einer App für universelle Geräte oder Mobilgeräte. Sie können eine Projektmappe mit einem freigegebenen Projekt erstellen und weiterhin Quellcode, Markupdateien und andere Assets und Ressourcen zwischen den beiden Projekten gemeinsam nutzen. Oder Sie können verschiedene Projektmappen erstellen und weiterhin dieselben Elemente mithilfe von Links teilen.

## Bei einer Windows 8.1-App

Portieren Sie das Projekt zu einer App für universelle Geräte oder Desktopgeräte. Wenn Sie die universelle Gerätefamilie wählen und Ihre App die APIs aufruft, die nur in der Desktopgerätefamilie implementiert sind, können Sie diese Aufrufe mit adaptivem Code schützen.

## Bei einer Windows Phone 8.1-App

Portieren Sie das Projekt zu einer App für universelle Geräte oder Mobilgeräte. Wenn Sie die universelle Gerätefamilie wählen und Ihre App die APIs aufruft, die nur in der Mobilgerätefamilie implementiert sind, können Sie diese Aufrufe mit adaptivem Code schützen.

## Anpassen der App an mehrere Formfaktoren

Die in den vorherigen Abschnitten gewählte Option bestimmt das Gerätespektrum, auf dem Ihre App oder Apps ausgeführt werden, und dabei kann es sich um eine sehr breite Palette von Geräten handeln. Selbst wenn Sie Ihre App auf mobile Geräte beschränken, müssen Sie dennoch ein breites Spektrum an Bildschirmgrößen unterstützen. Wenn Ihre App unter verschiedenen Formfaktoren ausgeführt wird, die sie früher nicht unterstützt haben, sollten Sie Ihre Benutzeroberfläche daher für diese Formfaktoren testen und ggf. Änderungen vornehmen, damit die Benutzeroberfläche entsprechend angepasst wird. Stellen Sie sich dies als eine Aufgabe nach der Portierung oder eine erweiterte Zielvorgabe für das Portieren vor. In den Fallstudien [Bookstore2](w8x-to-uwp-case-study-bookstore2.md) und [QuizGame](w8x-to-uwp-case-study-quizgame.md) gibt es einige Praxisbeispiele dafür.

## Portieren der einzelnen Ebenen

Beim Portieren einer universellen 8.1-App zum Modell für UWP-Apps können Sie auf nahezu alle Ihre Kenntnisse und Erfahrungen sowie einen Großteil Ihres Quellcodes, Markups und Ihrer Softwaremuster zurückgreifen.

-   **Ansicht**. Die Ansicht und das Ansichtsmodell bilden die UI Ihrer App. Im Idealfall besteht die Ansicht aus Markup, das an feststellbare Eigenschaften eines Ansichtsmodells gebunden ist. Ein weiteres gängiges, aber nur auf kurze Sicht zweckmäßiges Muster ist das direkte Ändern von UI-Elementen mit imperativem Code in einer CodeBehind-Datei. In beiden Fällen lassen sich Ihre UI-Markups und -Designs – und sogar imperativer Code zum Ändern von UI-Elementen – einfach portieren.
-   **Ansichtsmodelle und Datenmodelle**. Auch wenn Sie Muster zur Trennung der Zuständigkeiten (z. B. MVVM) nicht ausdrücklich anwenden, gibt es in Ihrer App zwangsläufig Code, der die Funktion des Ansichts- und Datenmodells übernimmt. Code für das Ansichtsmodell nutzt Typen in den Namespaces des Benutzeroberflächenframeworks. Sowohl der Code für das Ansichtsmodell als auch der Code für das Datenmodell nutzen zudem nicht visuelle Betriebssystem- und .NET Framework-APIs (darunter APIs für den Datenzugriff). Und diese APIs sind [auch für UWP-Apps verfügbar](https://msdn.microsoft.com/library/windows/apps/br211369), sodass der Großteil dieses Codes, wenn nicht gar der gesamte Code, ohne Änderung portiert wird.
-   **Clouddienste**. Höchstwahrscheinlich werden einige (oder sogar die meisten) Teile Ihrer App in Form von Diensten in der Cloud ausgeführt. Der auf dem Clientgerät ausgeführte Teil der App stellt eine Verbindung mit diesen Diensten her. Dies ist der Teil einer verteilten App, bei dem es am wahrscheinlichsten ist, dass er beim Portieren des Clientteils unverändert bleibt. Falls Sie noch keine Clouddienste nutzen, sind [Microsoft Azure Mobile Services](http://azure.microsoft.com/services/mobile-services/) eine gute Wahl für Ihre UWP-App. Sie bieten leistungsstarke Back-End-Komponenten, die Ihre App für verschiedenste Dienste aufrufen kann – angefangen bei einfachen Benachrichtigungen für Live-Kachelaktualisierungen bis hin zur komplexen Skalierbarkeit, die eine Serverfarm bereitstellen kann.

Überlegen Sie vor oder während der Portierung, ob Ihre App dadurch verbessert werden könnte, wenn Sie sie umgestalten und Code mit einem ähnlichen Zweck in Ebenen zusammenfassen, anstatt ihn willkürlich zu verteilen. Die Aufteilung Ihrer App in Ebenen wie die oben beschriebenen erleichtert Ihnen das Ausschließen von Fehlern, Testen und spätere Lesen und Warten Ihrer App. Sie können Funktionalität stärker wiederverwendbar machen, indem Sie dem Model-View-ViewModel ([MVVM](http://msdn.microsoft.com/magazine/dd419663.aspx))-Muster folgen. Dieses Muster trennt die Daten-, Geschäfts- und UI-Teile der App voneinander. Auch innerhalb der UI kann das Muster Zustand und Verhalten von den visuellen Elementen getrennt halten, sodass diese separat getestet werden können. Das MVVM-Muster bietet Ihnen die Möglichkeit, Ihre Daten und Geschäftslogik einmal zu schreiben und auf allen Geräten, unabhängig von der UI, zu verwenden. Es ist wahrscheinlich, dass Sie auch einen Großteil des Ansichtsmodells und der Ansichtselemente auf verschiedenen Geräten wiederverwenden können.

## Projekt für Microsoft Visual Studio 2015 RC

Besitzen Sie ein Windows 10-Projekt, das Sie mit Microsoft Visual Studio 2015 RC erstellt haben, finden Sie weitere Informationen unter [Aktualisieren Ihres UWP-Projekts von der RC-Version auf die RTM-Version von Microsoft Visual Studio 2015](update-your-visual-studio-2015-rc-project-to-rtm.md).
 
| Thema | Beschreibung |
|-------|-------------|
| [Portieren des Projekts](w8x-to-uwp-porting-to-a-uwp-project.md) | Sie haben zwei Möglichkeiten, wenn Sie mit dem Portierungsprozess beginnen. Eine Möglichkeit ist das Bearbeiten einer Kopie der vorhandenen Projektdateien, einschließlich des App-Paketmanifests (die Vorgehensweise finden Sie in den Informationen zum Aktualisieren der Projektdateien unter [Migrieren von Apps zur universellen Windows-Plattform (UWP)](https://msdn.microsoft.com/library/mt148501.aspx)). Die andere Möglichkeit ist das Erstellen eines neuen Windows 10-Projekts in Visual Studio und das Kopieren Ihrer Dateien in dieses Projekt. |
| [Problembehandlung](w8x-to-uwp-troubleshooting.md) | Wir empfehlen dringend, dieses Handbuch für das Portieren vollständig zu lesen. Wir wissen aber auch, dass Sie möglichst schnell die Phase erreichen möchten, in der Ihr Projekt erstellt und ausgeführt wird. Zu diesem Zweck können Sie einen temporären Fortschritt erzielen, indem Sie allen nicht unbedingt erforderlichen Code auskommentieren und anschließend zurückkehren, um die Schulden später zu bezahlen. Die Tabelle mit Symptomen und Möglichkeiten zur Problembehandlung in diesem Thema kann in dieser Phase hilfreich für Sie sein, ersetzt jedoch nicht das Lesen der nächsten Themen. Sie können die Tabelle jederzeit zu Rate ziehen, während Sie die weiteren Themen lesen. |
| [Portieren von XAML und UI](w8x-to-uwp-porting-xaml-and-ui.md) | Die Vorgehensweise zum Definieren einer Benutzeroberfläche in Form von deklarativem XAML-Markup lässt sich sehr gut von universellen 8.1-Apps auf UWP-Apps für die universelle Windows-Plattform (UWP) übertragen. Sie werden feststellen, dass der Großteil des Markups kompatibel ist. Unter Umständen sind jedoch einige Anpassungen bei Systemressourcenschlüsseln oder benutzerdefinierten Vorlagen erforderlich. |
| [Portieren: E/A, Gerät und App-Modell](w8x-to-uwp-input-and-sensors.md) | Code, der in das Gerät selbst integriert und auf dessen Sensoren abgestimmt ist, umfasst auch Eingaben vom und Ausgaben an den Benutzer. Auch die Datenverarbeitung kann einbezogen werden. Aber dieser Code wird in der Regel nicht als UI-Ebene oder Datenebene betrachtet. Dieser Code enthält die Integration in Vibrationscontroller, Beschleunigungsmesser, Gyroskop, Mikrofon und Lautsprecher (Überschneidungen mit Spracherkennung und Sprachsynthese), (geografischen) Standort und Eingabemodalitäten, z. B. Toucheingabe, Maus, Tastatur und Stift. |
| [Fallstudie: Bookstore1](w8x-to-uwp-case-study-bookstore1.md) | Dieses Thema enthält eine Fallstudie für das Portieren einer sehr einfachen universellen 8.1-App zu einer UWP-App für Windows 10. Bei einer universellen 8.1-App wird ein App-Paket für Windows 8.1 und ein anderes App-Paket für Windows Phone 8.1 erstellt. Mit Windows 10 können Sie ein einzelnes App-Paket erstellen, das Ihre Kunden auf einer Vielzahl von Geräten installieren können – und genau das werden wir in dieser Fallstudie tun. Weitere Informationen finden Sie unter [Anleitung für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn894631). |
| [Fallstudie: Bookstore2](w8x-to-uwp-case-study-bookstore2.md) | Diese Fallstudie baut auf den Informationen zum [SemanticZoom](https://msdn.microsoft.com/library/windows/apps/hh702601)-Steuerelement auf. Im Ansichtsmodell stellt jede Instanz der Author-Klasse die Gruppe der vom betreffenden Autor verfassten Titel dar. In SemanticZoom können wir dann entweder die Bücherliste nach Autoren gruppiert anzeigen oder die Liste verkleinern, um eine Sprungliste der Autoren zu erhalten. |
| [Fallstudie: QuizGame](w8x-to-uwp-case-study-quizgame.md) | Dieses Thema enthält eine Fallstudie zum Portieren eines funktionierenden Peer-zu-Peer-Quizspiels (WinRT 8.1-Beispiel-App) zu einer UWP-App für Windows 10. |

## Verwandte Themen

**Dokumentation**
* [Windows-Runtime-Referenz](https://msdn.microsoft.com/library/windows/apps/br211377)
* [Erstellen von universellen Windows-Apps für alle Windows-Geräte](http://go.microsoft.com/fwlink/p/?LinkID=397871)
* [Gestaltung der Benutzererfahrung für Apps](https://msdn.microsoft.com/library/windows/apps/hh767284)




<!--HONumber=Jun16_HO4-->


