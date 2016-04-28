---
description: Lese Liste, vermeiden dadurch Probleme, die Zertifizierung von Apps verhindern oder nach Veröffentlichung der App bei Stichprobenkontrolle auftreten können
title: Vermeiden allgemeiner Zertifizierungsfehler
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
---

# Vermeiden allgemeiner Zertifizierungsfehler


Lesen Sie diese Liste, und vermeiden Sie dadurch Probleme, die häufig die Zertifizierung von Apps verhindern oder nach der Veröffentlichung der App bei einer Stichprobenkontrolle auftreten können.

> **Hinweis**  Lesen Sie außerdem die [Windows Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/dn764944), um sicherzustellen, dass Ihre App alle darin aufgeführten Anforderungen erfüllt.

 

-   Reichen Sie die App erst ein, wenn sie fertig ist. Sie können die Beschreibung Ihrer App gern nutzen, um auf geplante Features hinzuweisen. Achten Sie jedoch darauf, dass Ihre App keine unvollständigen Abschnitte, Links zu unfertigen Webseiten oder andere Elemente enthält, die Kunden darauf schließen lassen, dass sich die App in einem unvollständigen Zustand befindet.

-   Testen Sie die App vor dem Einreichen mit dem [Zertifizierungskit für Windows-Apps](https://msdn.microsoft.com/library/windows/apps/mt186449).

-   Testen Sie die App unter verschiedenen Konfigurationen, um größtmögliche Stabilität sicherzustellen.

-   Vergewissern Sie sich, dass die App nicht abstürzt, wenn keine Netzwerkkonnektivität besteht! Auch wenn für die eigentliche Verwendung der App eine Verbindung erforderlich ist, muss sie richtig ausgeführt werden, wenn keine Verbindung besteht.
-   Formulieren Sie die Beschreibung der App so, dass sie den Funktionsumfang der App genau darstellt. Unterstützung erhalten Sie in den Richtlinien zum [Verfassen einer ansprechenden App-Beschreibung](write-a-great-app-description.md).

-   Achten Sie darauf, alle Fragen im Bereich [Altersfreigaben](age-ratings.md) vollständig und richtig zu beantworten.

-   Wenn die App die E-Commerce-APIs für den Windows Store aus dem [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197)-Namespace verwendet, müssen Sie die App testen und sich vergewissern, dass sie typische Ausnahmen behandelt. Stellen Sie außerdem sicher, dass die App die [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)-Klasse verwendet (und nicht die [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)-Klasse, die nur zu Testzwecken gedacht ist).

-   [Deklarieren Sie die App nur dann als barrierefrei](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), wenn Sie sie ausdrücklich für Barrierefreiheitsszenarien entwickelt und getestet haben.

-   [Stellen Sie unbedingt die erforderlichen Infos](notes-for-certification.md) zum Verwenden der App bereit, z. B. Benutzername und Kennwort für ein Testkonto, wenn sich Benutzer bei einem Dienst anmelden müssen, oder geben Sie die erforderlichen Schritte zum Zugreifen auf versteckte oder gesperrte Features an.

 

 






<!--HONumber=Mar16_HO1-->


