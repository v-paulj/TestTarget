---
author: mcleanbyron
Description: Vor dem Ausführen eines Experiments in Ihrer Universellen Windows-Plattform(UWP)-App mit A/B-Test müssen Sie Ihr Experiment im Dev Center-Dashboard definieren.
title: Definieren des Experiments im Dev Center-Dashboard
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
---

# Definieren des Experiments im Dev Center-Dashboard

Zum Ausführen eines Experiments in Ihrer Universellen Windows-Plattform (UWP)-App mit einem A/B-Test sollten Sie zu Beginn Ihr Experiment im Dev Center-Dashboard definieren.

Die folgenden Abschnitte beschreiben die allgemeinen Schritte zum Definieren eines Experiments im Dashboard. Eine exemplarische Vorgehensweise des gesamten Erstellungs- und Ausführungsprozesses eines Experiments finden Sie unter [Erstellen und Durchführen Ihres ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

## Erhalten Sie einen API-Schlüssel

Die ersten Schritte finden Sie unter der Seite **Experimente** des Dev Center-Dashboard, auf der Sie einen *API-Schlüssel* für Ihr Experiment erhalten.

Ein API-Schlüssel ist eine eindeutige ID, die Ihre App mit einem Experiment in Ihrem Dev Center-Konto verknüpft. Jedes Experiment ist genau mit einem API-Schlüssel verknüpft. Allerdings kann eine App über mehrere API-Schlüssel verfügen und jeder API-Schlüssel kann mit mehreren Experimenten verknüpft werden. Sie können API-Schlüssel verwenden, um zwischen verschiedenen Gruppen von Experimenten zu unterscheiden. Beispielsweise haben Sie möglicherweise einen Satz von Versuchen, den Sie für Tester in Ihrer Organisation freigeben, und einen anderen Satz von Versuchen, den Sie nur für externe Benutzer Ihrer App freigeben.

Sie müssen diesen API-Schlüssel verwenden, um eine Verbindung mit dem A/B-Testdienst im App-Code herzustellen. Weitere Informationen finden Sie unter [Code Ihrer App für Experimente](code-your-experiment-in-your-app.md).

1. Melden Sie sich beim [Dev Center-Dashboard](https://dev.windows.com/overview) an.
2. Unter **Ihre Apps** wählen Sie die App, für die Sie ein Experiment erstellen möchten.
3. Wählen Sie im Navigationsbereich **Dienste** und dann **Experimente**.
4. Der Abschnitt **API-Schlüssel** enthält einen Standard-API-Schlüssel für Ihre App mit dem Namen **API-Schlüssel #1**. Wenn Sie diesen Schlüssel verwenden möchten, geben Sie optional einen Anzeigenamen für diesen Schlüssel ein und kopieren Sie ihn zur Verwendung in Ihrem App-Code. Um einen neuen API-Schlüssel zu generieren, wählen Sie **Neuer API-Schlüssel** aus und geben Sie einen Anzeigenamen für den API-Schlüssel ein.

## Experiment erstellen

Erstellen Sie als Nächstes ein neues Experiment und definieren Sie die Ziele für das Experiment.

1. Klicken Sie im Abschnitt **Experimente** auf die Schaltfläche **Neues Experiment**.
2. Wählen Sie im Abschnitt **API-Schlüsselnamen** den API-Schlüssel aus, den Sie mit diesem Experiment verknüpfen möchten. Wenn Sie nur einen API-Schlüssel haben, wird dieser API-Schlüssel standardmäßig ausgewählt.
3. Geben Sie im Feld **Name des Experiments** einen Namen ein, mit dem Sie das Experiment leicht ermitteln können. Nach der Erstellung eines Experiments erscheint dieser Name in der Liste der entworfenen, aktiven und abgeschlossenen Experimente auf der **Experimente**-Seite.
4. Wenn Sie ein Testexperiment erstellen möchten, klicken Sie auf das Kontrollkästchen **Testexperiment**. Der Unterschied zwischen Testexperimenten und regulären Experimenten liegt darin, dass nur Testexperimente nach erfolgter Aktivierung noch geändert werden können.

  Testexperimente sollen Ihnen dabei helfen, alle Variationen auf einem Client-Gerät zu testen, bevor Sie Ihr Experiment für Kunden freigeben. Um sicherzustellen, dass eine Variante erwartungsgemäß auf Clients bereitgestellt wird, können Sie ein Testexperiment mit 100 % Verteilung für eine Variante und 0 % für andere Varianten aktivieren. Nachdem Sie diese Variante überprüft haben, können Sie den Prozess für andere Varianten wiederholen.
  > **Hinweis**  Überprüfen Sie dieses Kontrollkästchen nur, wenn Sie ein Testexperiment zum Überprüfen von Parametern über interne Tests erstellen. Aktivieren Sie dieses Kontrollkästchen nicht, wenn Sie eine Experiment erstellen, das Sie an Kunden freigegeben werden.

5. Geben Sie im Feld **Ereignisnamen anzeigen** den Namen des *Anzeigeereignisses* für Ihr Experiment ein. Das Anzeigeereignis ist eine beliebige Zeichenfolge, die eine Aktivität darstellt, wenn der Benutzer beginnt, eine Variante anzusehen, die Teil Ihres Experiments ist. Der App-Code sendet die Zeichenfolge des Anzeigeereignisses an Dev Center, wenn der Benutzer beginnt, eine Variante anzusehen. Weitere Informationen finden Sie unter [Code Ihrer App für Experimente](code-your-experiment-in-your-app.md).
6. Definieren Sie im Abschnitt **Ziele und Umwandlungsereignisse** mindestens ein Ziel für Ihr Experiment:
  * Geben Sie im Feld **Name des Ziels** einen beschreibenden Namen für Ihr Ziel ein. Nach dem Ausführen eines Experiments erscheint dieser Name in der Ergebniszusammenfassung des Experiments.
  * Geben Sie im Feld **Ereignisnamenumwandlung** den Namen des *Umwandlungsereignisses* für dieses Ziel ein. Eine Ereignisumwandlung ist eine beliebige Zeichenfolge, die ein Ziel für dieses Ziel darstellt. Ihr App-Code sendet diese Umwandlungsereignis-Zeichenfolge an Dev Center, wenn der Benutzer ein Ziel erreicht. Weitere Informationen finden Sie unter [Code Ihrer App für Experimente](code-your-experiment-in-your-app.md).
  * Wählen Sie im Feld **Ziel** **Maximieren** oder **Minimieren**aus, je nachdem, ob Sie das Vorkommen des Umwandlungsereignisses maximieren oder minimieren möchten. Diese Informationen werden in der Ergebniszusammenfassung des Experiments verwendet.

  >**Hinweis** Dev Center meldet nur das erste Umwandlungsereignis für jede Benutzeransicht innerhalb eines Zeitraums von 24 Stunden. Wenn ein Benutzer innerhalb von 24 Stunden mehrere Umwandlungsereignisse in Ihrer App auslöst, wird nur das erste Umwandlungsereignis gemeldet. So soll verhindert werden, dass die Experimentergebnisse für eine Stichprobengruppe von Benutzern durch einen einzelnen Benutzer verfälscht werden, wenn das Ziel darin besteht, die Anzahl der Benutzer zu maximieren, die eine Umwandlung durchführen.

## Definieren Sie die Varianten und Einstellungen für das Experiment

Als Nächstes definieren Sie die Varianten und Standardeinstellungen für Ihr Experiment.

* Eine *Variante* ist eine Sammlung von einer Einstellung oder mehreren Einstellungen, die Sie in Ihrem Experiment testen. Jedes Experiment muss mindestens eine Einstellung und zwei Varianten umfassen (einschließlich des Steuerelements). Ein Experiment kann bis zu fünf Variationen aufweisen.
* Eine *Einstellung* ist ein Wert, den Ihre App verwendet, um eine Programmvariable zu initialisieren. Während des Experiments ändert sich der Wert der Einstellung von Variante zu Variante. Nachdem Sie das Experiment beenden, wird die Einstellung dem Wert der Variante zugewiesen, den Sie für die Freigabe für alle Benutzer Ihrer App auswählen. Einstellungen können die folgenden Typen aufweisen: eine Zeichenfolge, Boolean, Double und ganze Zahl.

Definition der Varianten und Standardeinstellungen für Ihr Experiment:
1. Im Abschnitt **Varianten und Einstellungen** sollten Ihnen zwei standardmäßige Varianten angezeigt werden: **Variante A (Steuerelement)** und **Variante B**. Wenn Sie mehrere Varianten möchten, klicken Sie auf **Variante hinzufügen**. Optional können Sie jede Variante umbenennen.
2. Als Nächstes erstellen Sie die Einstellungen für Ihre Varianten. Klicken Sie auf **Einstellung hinzufügen**, um jede neue Einstellung zu erstellen, und geben Sie den Einstellungsnamen und den Wert der Einstellung in jeder Variante ein.
3. Standardmäßig werden Variationen gleichmäßig an Ihre App-Benutzer verteilt. Wenn Sie einen bestimmten Verteilungsprozentsatz auswählen möchten, deaktivieren Sie das Kontrollkästchen **Gleichmäßig verteilen** und geben Sie die Prozentsätze in die Zeile **Verteilung (%)** ein.

## Speichern Sie Ihr Experiment

Wenn Sie die Eingabe in die erforderlichen Felder für Ihr Experiment abgeschlossen haben, klicken Sie auf **Speichern**, um Ihr Experiment zu speichern.

> **Wichtig** Nach dem Speichern eines Experiments können Sie den API-Schlüssel für das Experiment nicht mehr ändern, auch wenn Sie das Experiment noch nicht aktiviert haben.

Wenn Sie mit den Parametern für Ihr Experiment zufrieden sind Sie bereit sind, es zu aktivieren, damit Sie mit der Datenerfassung von Ihrer App beginnen können, klicken Sie auf **Aktivieren**. Wenn das Experiment aktiv ist, kann Ihre App Variationseinstellungen abrufen und Anzeige- und Umwandlungsereignisse im Dev Center melden.

> **Wichtig** Nach der Aktivierung eines Experiments können Sie die Experimentparameter nicht mehr ändern, sofern es sich nicht um ein Testexperiment handelt (d. h. Sie haben beim Erstellen des Experiments das Kontrollkästchen **Testexperiment** aktiviert). Es wird empfohlen, das Experiment vor der Aktivierung in der App zu codieren.

## Nächste Schritte

Nachdem Sie Ihr Experiment im Dev Center-Dashboard definiert haben, sind Sie bereit für die folgenden Schritte:
1. [Codieren Sie Ihre App für das Experiment](code-your-experiment-in-your-app.md). Verwenden Sie eine API im Microsoft Store Engagement and Monetization SDK, um Varianteneinstellungen für das Experiment abzurufen, ändern Sie mit diesen Daten das Verhalten des getesteten Features, und senden Sie das Anzeigeereignis und die Umwandlungsereignisse an Dev Center.
2. [Führen Sie das Experiment im Dev Center-Dashboard aus, und verwalten Sie es](manage-your-experiment.md). Im Dashboard können Sie die Ergebnisse des Experiments überprüfen und das Experiment abschließen.

## Verwandte Themen

  * [Schreiben Sie den Code der App für Experimente](code-your-experiment-in-your-app.md)
  * [Verwalten des Experiments im Dev Center-Dashboard](manage-your-experiment.md)
  * [Erstellen und Ausführen eines ersten Experiments mit A/B-Test](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Ausführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)


<!--HONumber=May16_HO2-->


