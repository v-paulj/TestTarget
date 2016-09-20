---
author: jnHs
Description: "Im Abschnitt „App-Deklarationen“ der Seite „App-Eigenschaften“ können Sie während des Übermittlungsprozesses zusätzliche Informationen zu Ihrer App bereitstellen."
title: App-Deklarationen
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ee2e54494a868fbec8c4a8bd7bbb9bdcd6cbf9ae

---

# App-Deklarationen

Im Abschnitt **App-Deklarationen** der Seite **App-Eigenschaften** können Sie während des [Übermittlungsprozesses](app-submissions.md) zusätzliche Informationen zu Ihrer App bereitstellen. Diese Deklarationen können dazu beitragen, dass Ihre App ordnungsgemäß angezeigt und der richtigen Zielgruppe angeboten wird. Sie können den Kunden darüber hinaus auch mitteilen, wie Ihre App verwendet wird.

Neben den einzelnen Deklarationen wird in den folgenden Abschnitten beschrieben, was Sie bei der Entscheidung, ob sich eine Deklaration auf Ihre App bezieht, berücksichtigen sollten.

## Diese App ermöglicht es Benutzern, Einkäufe zu tätigen, verwendet jedoch nicht das Windows Store-Commerce-System.

Für die meisten Apps sollte dieses Feld deaktiviert bleiben, da Apps, die Möglichkeiten für In-App-Einkäufe bieten, im Allgemeinen die API für In-App-Einkäufe von Microsoft zur Erstellung und zum [Einreichen der IAPs](iap-submissions.md) verwenden. Gemäß der [Vereinbarung für App-Entwickler](https://msdn.microsoft.com/library/windows/apps/hh694058) können Apps, die vor dem 29. Juni 2015 erstellt und eingereicht wurden, die In-App-Einkauffunktionalität möglicherweise weiterhin anbieten, ohne die Handelsplattform von Microsoft zu verwenden. Dazu muss die Einkaufsfunktionalität jedoch die [Windows Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_8) erfüllen. Wenn dies auf Ihre App zutrifft, müssen Sie dieses Kontrollkästchen aktivieren. Lassen Sie das Kontrollkästchen andernfalls deaktiviert.

## Diese App wurde auf Einhaltung der Richtlinien zur Barrierefreiheit getestet.

Wenn Sie dieses Kontrollkästchen aktivieren, kann die App von Kunden gefunden werden, die im Store speziell nach barrierefreien Apps suchen.

Sie sollten dieses Kontrollkästchen erst aktivieren, nachdem Sie die folgenden Schritte erledigt haben:

-   Alle relevanten Barrierefreiheitsinfos für UI-Elemente festlegen, z.B. barrierefreie Namen
-   Tastaturnavigation und -vorgänge unter Berücksichtigung der Registerkartenreihenfolge, Tastaturaktivierung, Navigation mit Pfeiltasten und Tastenkombinationen implementieren
-   Barrierefreie visuelle Darstellung sicherstellen, z. B. durch Berücksichtigen eines Textkontrastverhältnisses von 4,5:1 (und nicht nur durch die Verwendung farblich gekennzeichneter Informationen)
-   Tools zum Testen der Barrierefreiheit verwenden, z.B. Inspect oder AccChecker, um Ihre App zu überprüfen und alle von diesen Tools ermittelten Fehler mit hoher Priorität zu beheben
-   Die wichtigsten Szenarien der App unter Verwendung von Funktionen und Tools wie „Sprachausgabe“, „Bildschirmlupe“, „Bildschirmtastatur“, „Hoher Kontrast“ und „Hoher DPI-Wert“ vollständig überprüfen

Wenn Sie Ihre App als barrierefrei ausweisen, erklären Sie ausdrücklich, dass Ihre App eine Barrierefreiheit für alle Kunden aufweist, einschließlich für Personen mit Behinderungen. Das bedeutet beispielsweise, dass Sie die App im Modus mit hohem Kontrast und die Sprachausgabe getestet haben. Sie haben außerdem sichergestellt, dass die Benutzeroberfläche ordnungsgemäß mit der Tastatur, der Bildschirmlupe und weiteren Tools zur Unterstützung der Barrierefreiheit funktioniert.

Weitere Informationen finden Sie unter [Barrierefreiheit für Windows-Runtime-Apps](https://msdn.microsoft.com/library/windows/apps/dn263101), [Barrierefreiheitstests](https://msdn.microsoft.com/library/windows/apps/mt297664) und [Barrierefreiheit im Store](https://msdn.microsoft.com/library/windows/apps/mt297663).

> 
            **Wichtig**  Weisen Sie die App nur dann als barrierefrei aus, wenn Sie sie ausdrücklich für diesen Zweck entwickelt und getestet haben. Falls Ihre App als barrierefrei ausgewiesen ist, jedoch eigentlich keine Barrierefreiheit unterstützt, werden Sie wahrscheinlich negatives Feedback von der Community erhalten.

## Kunden können diese App auf alternativen Laufwerken oder Wechselmedien installieren.

Dieses Kontrollkästchen ist standardmäßig aktiviert, damit Kunden Ihre App auf Wechselmedien wie etwa einer SD-Karte oder auf einem systemfremden Volumelaufwerk wie einem externen Laufwerk installieren können.

Wenn Sie verhindern möchten, dass Ihre App auf alternativen Laufwerken oder Wechselmedien installiert wird, deaktivieren Sie dieses Kontrollkästchen.

Beachten Sie, dass keine Option zum Einschränken der Installation einer App auf Wechselmedien vorhanden ist.

> 
            **Hinweis**  Für Windows Phone 8.1 wurde dies zuvor über die Datei „StoreManifest.xml“ angegeben.

## Windows kann die Daten dieser App in automatische Sicherungen auf OneDrive einschließen.

Dieses Kontrollkästchen ist standardmäßig aktiviert, damit die Daten Ihrer App eingeschlossen werden können, wenn ein Kunde automatische OneDrive-Sicherungen von Windows erstellen lässt.

Wenn Sie verhindern möchten, dass die App-Daten in automatische Sicherungen eingeschlossen werden, deaktivieren Sie das Kontrollkästchen.

> 
            **Hinweis**  Für Windows Phone 8.1 wurde dies zuvor über die Datei „StoreManifest.xml“ angegeben.

 

 

 







<!--HONumber=Jun16_HO4-->


