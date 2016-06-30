---
author: mcleanbyron
Description: "Sie können über das Windows Dev Center-Dashboard Experimente für Ihre UWP-Apps (Universelle Windows-Plattform) mit A/B-Tests durchführen."
title: "Ausführen von Experimenten mit A/B-Tests"
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 88fd0516e3c10b657884b93377480b62c1758992

---

# Ausführen von Experimenten mit A/B-Tests

Sie können über das Windows Dev Center-Dashboard Experimente für Ihre UWP-Apps (Universelle Windows-Plattform) mit A/B-Tests durchführen.

Bei einem A/B-Test experimentieren Sie mit Zuweisungen von Programmvariablen in Ihren Apps über das Dev Center. Indem Sie App-Logik im Hinblick auf mit A/B-Tests verwendbare Programmvariablen erstellen, können Sie Varianten Ihrer App für zufällig ausgewählte Zielgruppensegmente aktivieren. Das Ziel eines A/B-Tests besteht darin, eine Variante zu identifizieren, die wahrscheinlich zu besseren Konversionsraten führt (z. B. mehr In-App-Käufe).

Wenn Sie eine Variante ermittelt haben, die für Ihre Geschäftsziele am besten geeignet ist, können Sie das Experiment sofort beenden und diese Variante für die gesamte Zielgruppe über das Dev Center-Dashboard aktivieren, ohne die App erneut veröffentlichen zu müssen.

## Erstellen und Ausführen einen A/B-Tests

Führen Sie zum Erstellen und Ausführen eines A/B-Tests die folgenden Schritte aus:

1. [Definieren Sie das Experiment im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md). Jedes Experiment besteht aus folgenden Elementen:
  * Ein *Anzeigeereignis*, das angibt, wann der Benutzer mit dem Anzeigen einer Variante beginnt, die Teil des Experiments ist.
  * Eines oder mehrere Ziele mit *Umwandlungsereignissen*, um anzugeben, wann ein Ziel erreicht wurde.
  * Eine oder mehrere *Varianten*, die die im Experiment verwendeten Einstellungen definieren.
2. [Codieren Sie Ihre App für das Experiment](code-your-experiment-in-your-app.md). Verwenden Sie eine API im Microsoft Store Engagement and Monetization SDK, um Varianteneinstellungen für das Experiment abzurufen, ändern Sie mit diesen Daten das Verhalten des getesteten Features, und senden Sie das Anzeigeereignis und die Umwandlungsereignisse an Dev Center.
3. [Führen Sie das Experiment im Dev Center-Dashboard aus, und verwalten Sie es](manage-your-experiment.md). Im Dashboard können Sie die Ergebnisse des Experiments überprüfen und das Experiment abschließen.

Eine exemplarische Vorgehensweise, in der der Prozess vollständig dargestellt wird, finden Sie unter [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md) (Erstellen und Ausführen eines ersten Experiments mit A/B-Tests).

## Anforderungen

A/B-Tests im Windows Dev Center werden nur für UWP-Apps unterstützt.

Damit Sie Experimente mit A/B-Tests ausführen können, müssen Sie den Entwicklungscomputer einrichten:

* Führen Sie [diese Anweisungen](../get-started/get-set-up.md) aus, um den Entwicklungscomputer für die UWP-Entwicklung einzurichten.
* Installieren Sie das [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk). Neben der API für Experimente bietet dieses SDK APIs für andere Features, z. B. das Anzeigen von Werbung und das Weiterleiten von Kunden zum Feedback-Hub, um Feedback zur App zu erfassen. Weitere Informationen zu diesem SDK finden Sie unter [Monetize your app with the Store Engagement and Monetization SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) (Monetisieren Ihrer App mit dem Store Engagement and Monetization SDK).

## Bewährte Methoden

Um möglichst aussagekräftige Ergebnisse zu erzielen, sollten Sie beim Durchführen von Experimenten mit A/B-Tests diese Empfehlungen berücksichtigen:

* Führen Sie Experimente mit nur zwei Varianten mit einer zufälligen 50/50-Aufteilung für Variantenzuweisungen durch.
* Führen Sie die Experimente mindestens 2 bis 4 Wochen lang durch, um ausreichende Daten zu erfassen, die statistisch signifikant und aussagekräftig sind.

## Verwandte Themen

* [Definieren des Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
* [Codieren der App für das Experiment](code-your-experiment-in-your-app.md)
* [Verwalten des Experiments im Dev Center-Dashboard](manage-your-experiment.md)
* [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


