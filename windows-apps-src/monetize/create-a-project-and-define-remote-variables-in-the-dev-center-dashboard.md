---
author: mcleanbyron
Description: "Bevor Sie in Ihrer App für die universelle Windows-Plattform (UWP) ein Experiment mit A/B-Tests ausführen können, müssen Sie ein Projekt erstellen und Ihre Remotevariablen im Dev Center-Dashboard definieren."
title: Erstellen eines Projekts und Festlegen von Remotevariablen im Dev Center-Dashboard
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: ea455892717546f7f789054609664ca1c0017699

---

# Erstellen eines Projekts und Festlegen von Remotevariablen im Dev Center-Dashboard

Erstellen Sie zunächst für das Experiment ein [Projekt](run-app-experiments-with-a-b-testing.md#terms) für Ihre App im Dev Center-Dashboard und definieren Sie die Remotevariablen, auf die Ihre App zugreifen kann.

1. Melden Sie sich beim [Dev Center-Dashboard](https://dev.windows.com/overview) an.
2. Wählen Sie unter **Ihre Apps** die App aus, für die Sie ein Experiment erstellen möchten.
3. Wählen Sie im Navigationsbereich **Dienste** und dann **Experimente**.
4. Klicken Sie auf der Seite **Experimente** im Abschnitt **Projekte** auf die Schaltfläche **Neues Projekt**. Wenn Sie bereits ein oder mehrere Projekte erstellt haben, werden diese Projekte im Abschnitt **Projekte** aufgeführt.
5. Geben Sie auf der Seite **Neues Projekt** einen Namen für das neue Projekt ein.
6. Fügen Sie im Abschnitt **Remotevariablen** die [Variablen](run-app-experiments-with-a-b-testing.md#terms) hinzu, die für alle Experimente in diesem Projekt verfügbar sein sollen, und definieren Sie die Standardwerte für jede Variable. Die hier festgelegten Standardwerte werden für die Steuerelementgruppe der Experimente verwendet, sowie für alle Benutzer, die an dem Experiment nicht teilnehmen.
  1. Falls der Abschnitt **Remotevariablen** reduziert ist, klicken Sie in der Abschnittsüberschrift auf **Anzeigen**.
  2. Klicken Sie auf **Variable hinzufügen**, um jede neue Variable zu erstellen, die für jedes Experiment in diesem Projekt verfügbar sein soll, und geben Sie den Variablennamen und den Standardwert der Variablen ein.
  3. Wenn Sie das Hinzufügen von Variablen beendet haben, klicken Sie auf **Speichern**.
3. Notieren Sie im Abschnitt **SDK-Integration** den Wert der [Projekt-ID](run-app-experiments-with-a-b-testing.md#terms). Zum [Codieren der App für das Experiment](code-your-experiment-in-your-app.md), müssen Sie auf diese Projekt-ID in Ihrem Code verweisen, damit Sie Variantendaten empfangen sowie Anzeige- und Umwandlungsereignisse an das Dev Center melden können.

>**Hinweis**&nbsp;&nbsp;Sie können keine Remotevariablen bearbeiten, hinzufügen oder entfernen, während ein Experiment im Projekt aktiv ist. Diese Einschränkung hilft, die Integrität der Daten der Steuerelementgruppe für das aktive Experiment zu schützen.


## Nächste Schritte

Nachdem Sie ein Projekt erstellt haben, können Sie mit dem [Codieren der App für das Experiment](code-your-experiment-in-your-app.md) beginnen, indem Sie Werte von Remotevariablen in Ihrer App abrufen, und Sie können [ein Experiment im Projekt erstellen](define-your-experiment-in-the-dev-center-dashboard.md).

## Verwandte Themen

* [Codieren der App für das Experiment](code-your-experiment-in-your-app.md)
* [Definieren des Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
* [Verwalten des Experiments im Dev Center-Dashboard](manage-your-experiment.md)
* [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ausführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Aug16_HO5-->


