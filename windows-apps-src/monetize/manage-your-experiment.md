---
author: mcleanbyron
Description: "Nachdem Sie Ihr Experiment im Dev Center-Dashboard definiert und in Ihrer App programmiert haben, können Sie das Experiment aktivieren und das Dev Center-Dashboard zum Prüfen der Ergebnisse Ihres Experiments verwenden."
title: Verwalten des Experiments im Dev Center-Dashboard
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 24ca106cc83c4495657972f463c556585cdfcb45

---

# Verwalten des Experiments im Dev Center-Dashboard

Nachdem Sie [Ihr Experiment im Dev Center-Dashboard definiert](define-your-experiment-in-the-dev-center-dashboard.md) und [in Ihrer App programmiert haben](code-your-experiment-in-your-app.md), können Sie das Experiment aktivieren und das Dev Center-Dashboard zum Prüfen der Ergebnisse Ihres Experiments verwenden. Nach Abrufen aller benötigten Daten können Sie Ihr Experiment beenden und festlegen, ob die Einstellungen für die Variationssteuerung in allen Apps oder nur in einer der Variationen verwendet werden.

> 
            **Hinweis** Wenn Sie ein Experiment aktivieren, beginnt Dev Center umgehend mit der Erfassung von Daten aus allen Apps, die zum Protokollieren von Daten für Ihr Experiment instrumentiert sind. Bis zur Anzeige von Experimentdaten im Dashboard können jedoch mehrere Stunden vergehen.

Eine exemplarische Vorgehensweise, die den gesamten Erstellungs- und Ausführungsprozess für ein Experiment veranschaulicht, finden Sie unter [Erstellen und Durchführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

## Aktivieren Ihres Experiments

Wenn Sie mit den Parametern für Ihr Experiment im Dashboard zufrieden sind und den App-Code aktualisiert haben, können Sie Ihr Experiment aktivieren, damit mit der Erfassung der Experimentdaten aus Ihrer App begonnen wird. Wenn das Experiment aktiv ist, kann Ihre App Variationseinstellungen abrufen und Anzeige- und Umwandlungsereignisse im Dev Center melden.

1. Melden Sie sich beim [Dev Center-Dashboard](https://dev.windows.com/overview) an.
2. Wählen Sie unter **Ihre Apps** die App mit dem Experiment, das Sie aktivieren möchten.
3. Wählen Sie im Navigationsbereich **Dienste** und dann **Experimentation**.
4. Der Abschnitt **Experimente** enthält Experimententwürfe, aktive und beendete Experimente für die aktuelle App. Klicken Sie auf den Filter **Entwurf**, und klicken Sie dann für das Experiment, das Sie aktivieren möchten, auf **Aktivieren**.

> 
            **Wichtig**  Nach der Aktivierung eines Experiments können Sie die Experimentparameter nicht mehr ändern, wenn es sich nicht um ein Testexperiment handelt (d.h. Sie haben beim Erstellen des Experiments das Kontrollkästchen **Testexperiment** aktiviert). Es wird empfohlen, das Experiment vor der Aktivierung in der App zu codieren.


## Prüfen der Experimentergebnisse

1. Kehren Sie in Dev Center zur Seite **Experimentation** für Ihre App zurück.
2. Klicken Sie im Abschnitt **Experimente** auf den Filter **Aktiv**, und klicken Sie dann auf den Namen des aktiven Experiments, um zur Experimentseite zu wechseln.
3. Bei aktiven oder beendeten Experimenten enthalten die ersten zwei Abschnitte auf dieser Seite die Ergebnisse Ihres Experiments:
  * Der Abschnitt **Ergebniszusammenfassung** enthält die Experimentziele und die Umwandlungsquote für jede Variation.
  * Der Abschnitt **Ergebnisdetails** enthält ausführlichere Informationen zu den Experimentzielen, u. a. Anzeige-, Umwandlungsereignisse, Umwandlungsquote, Delta in %, Konfidenz und Signifikanz. Die *Konfidenz* ist ein statistisches Maß zum Angeben der Zuverlässigkeit einer Schätzung, mit dem die Fehlerspanne ermittelt wird. Die *Signifikanz* ist ein statistisches Maß, mit dem basierend auf einer Stichprobe die Wahrscheinlichkeit dafür bestimmt wird, dass ein Ergebnis nicht zufällig ist, sondern auf eine bestimmte Ursache zurückzuführen ist.

  >
            **Hinweis** Dev Center meldet nur das erste Umwandlungsereignis für jeden Benutzer innerhalb eines Zeitraums von 24 Stunden. Wenn ein Benutzer innerhalb von 24 Stunden mehrere Umwandlungsereignisse in Ihrer App auslöst, wird nur das erste Umwandlungsereignis gemeldet. So soll verhindert werden, dass die Experimentergebnisse für eine Stichprobengruppe von Benutzern durch einen einzelnen Benutzer mit mehreren Umwandlungsereignissen verfälscht wird.


## Beenden Ihres Experiments

1. Kehren Sie Im Dashboard zur Experimentseite zurück. Einzelheiten dazu finden Sie im vorherigen Abschnitt.
2. Führen Sie im Abschnitt **Ergebniszusammenfassung** eine der folgenden Aktionen durch:
  * Klicken Sie zum Beenden des Experiments und weiteren Verwenden der Einstellungen in der Steuerungsvariation in Ihrer App auf **Beibehalten**.
  * Klicken Sie zum Beenden des Experiments und zum Verwenden der Einstellungen in einer anderen Variation Ihrer App unter der Variation, zu der Sie wechseln möchten, auf **Wechseln**.
3. Klicken Sie auf **OK**, um zu bestätigen, dass Sie das Experiment beenden möchten.


## Verwandte Themen

  * [Definieren des Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Programmieren der App für Experimente](code-your-experiment-in-your-app.md)
  * [Erstellen und Durchführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Durchführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


