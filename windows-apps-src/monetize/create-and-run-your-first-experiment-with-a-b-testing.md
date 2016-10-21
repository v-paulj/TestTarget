---
author: mcleanbyron
Description: "In dieser exemplarischen Vorgehensweise wird Ihr erstes Experiment mit A/B-Tests erstellt, ausgeführt und verwaltet."
title: "Erstellen und Ausführen eines ersten Experiments mit A/B-Tests"
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
translationtype: Human Translation
ms.sourcegitcommit: bfe4862c441ca095a40df4f594fdf9b3e213d142
ms.openlocfilehash: ab15741531b829c496811cdfca35059cd113f91d

---

# Erstellen und Ausführen eines ersten Experiments mit A/B-Tests

In dieser exemplarischen Vorgehensweise führen Sie folgende Aktionen aus:
* Erstellen eines [Projekts](run-app-experiments-with-a-b-testing.md#terms) für das Experiment im Dev Center-Dashboard, das verschiedene Remotevariablen festlegt, die den Text und die Farbe einer App-Schaltfläche darstellen
* Erstellen einer App mit Code, die die Werte von Remotevariablen abruft, anhand dieser Daten die Hintergrundfarbe einer Schaltfläche ändert und Anzeige- sowie Umwandlungsereignisdaten wieder in Dev Center protokolliert
* Erstellen eines Experiments im Projekt, um zu testen, ob die Anzahl von Klicks auf eine App-Schaltfläche durch das Ändern der Hintergrundfarbe der Schaltfläche erfolgreich erhöht werden kann
* Ausführen der App, um Experimentdaten zu sammeln
* Überprüfen der Experimentergebnisse im Dev Center-Dashboard, Auswählen einer Variante zur Aktivierung für alle Benutzer der App und Abschließen des Experiments

Eine Übersicht über A/B-Tests mit Dev Center finden Sie unter [Ausführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md).

## Voraussetzungen

Zum Abschließen dieser exemplarischen Vorgehensweise benötigen Sie ein Windows Dev Center-Konto und müssen Ihren Entwicklungscomputer gemäß der Beschreibung in [Ausführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md) konfigurieren.

## Erstellen eines Projekts mit Remotevariablen in Windows Dev Center

1. Melden Sie sich beim [Dev Center-Dashboard](https://dev.windows.com/overview) an.
2. Wenn Sie eine bereits in Dev Center vorhandene App zu Erstellen eines Experiments verwenden möchten, wählen Sie diese App im Dashboard aus. Wenn Ihr Dashboard noch keine App enthält, [erstellen Sie eine neue App durch Reservierung eines Namens](../publish/create-your-app-by-reserving-a-name.md) und wählen diese App dann im Dashboard aus.
3. Klicken Sie im Navigationsbereich auf **Dienste** und dann auf **Experimentation**.
4. Klicken Sie auf der nächsten Seite im Abschnitt **Projekte** auf die Schaltfläche **Neues Projekt**.
5. Geben Sie auf der Seite **Neues Projekt** den Namen **Button Click Experiments** für das neue Projekt ein.
6. Erweitern Sie den Abschnitt **Remotevariablen**, und klicken Sie viermal auf **Variable hinzufügen**. Sie sollten jetzt über vier leere Variablenzeilen verfügen.
  * Geben Sie in der ersten Zeile **buttonText** als Variablennamen und **Graue Schaltfläche** in der Spalte **Standardwert** ein.
  * Geben Sie in der zweiten Zeile **r** als Variablennamen und **128** in der Spalte **Standardwert** ein.
  * Geben Sie in der dritten Zeile **g** als Variablennamen und **128** in der Spalte **Standardwert** ein.
  * Geben Sie in der vierten Zeile **b** als Variablennamen und **128** in der Spalte **Standardwert** ein.
7. Klicken Sie auf **Speichern**, und notieren Sie den Wert der [Projekt-ID](run-app-experiments-with-a-b-testing.md#terms) im Abschnitt **SDK-Integration**. Im nächsten Abschnitt aktualisieren Sie den App-Code und verweisen im Code auf diesen Wert.

## Codieren des Experiments in der App

1. Erstellen Sie in Visual Studio2015 ein neues UWP-Projekt (Universelle Windows-Plattform) mit Visual C#. Geben Sie dem Projekt den Namen **SampleExperiment**.
2. Erweitern Sie im Projektmappen-Explorer den Projektknoten, klicken Sie mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Verweis hinzufügen**.
3. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.
4. Aktivieren Sie in der Liste mit den SDKs das Kontrollkästchen neben **Microsoft Engagement Framework**, und klicken Sie anschließend auf **OK**.
5. Doppelklicken Sie im **Projektmappen-Explorer** auf „MainPage.xaml“, um den Designer für die Hauptseite in der App zu öffnen.
6. Ziehen Sie eine **Schaltfläche** aus der **Toolbox** auf die Seite.
7. Doppelklicken Sie im Designer auf die Schaltfläche, um die Codedatei zu öffnen, und fügen Sie einen Ereignishandler für das **Click**-Ereignis hinzu.  
8. Ersetzen Sie den gesamten Inhalt der Codedatei mit folgendem Code. Weisen Sie die ```projectId```-Variable dem Wert der [Projekt-ID](run-app-experiments-with-a-b-testing.md#terms) zu, den Sie im vorhergehenden Abschnitt aus dem Dev Center-Dashboard abgerufen haben.

  ```CSharp
  using System;
  using Windows.UI.Xaml;
  using Windows.UI.Xaml.Controls;
  using Windows.UI.Xaml.Media;
  using System.Threading.Tasks;
  using Windows.UI;
  using Windows.UI.Core;

  // Namespace for A/B testing.
  using Microsoft.Services.Store.Engagement;

  namespace SampleExperiment
  {  
     public sealed partial class MainPage : Page
     {
        private StoreServicesExperimentVariation variation;
        private StoreServicesCustomEventLogger logger;

        // Assign this variable to the project ID for your experiment from Dev Center.
        private string projectId = "";

        public MainPage()
        {
            this.InitializeComponent();

            // Because this call is not awaited, execution of the current method
            // continues before the call is completed.
#pragma warning disable CS4014
            InitializeExperiment();
#pragma warning restore CS4014
        }

        private async Task InitializeExperiment()
        {
            // Get the current cached variation assignment for the experiment.
            var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(projectId);
            variation = result.ExperimentVariation;

            // Check whether the cached variation assignment needs to be refreshed.
            // If so, then refresh it.
            if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
            {
                result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

                // If the call succeeds, use the new result. Otherwise, use the cached value.
                if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
                {
                    variation = result.ExperimentVariation;
                }
            }

            // Get remote variables named "buttonText", "r", "g", and "b" from the variation
            // assignment. If no variation assignment is available, the variables default
            // to "Grey button" for the button text and grey RGB value for the button color.
            var buttonText = variation.GetString("buttonText", "Grey Button");
            var r = (byte)variation.GetInt32("r", 128);
            var g = (byte)variation.GetInt32("g", 128);
            var b = (byte)variation.GetInt32("b", 128);

            // Assign button text and color.
            await button.Dispatcher.RunAsync(
                CoreDispatcherPriority.Normal,
                () =>
                {
                    button.Background = new SolidColorBrush(Color.FromArgb(255, r, g, b));
                    button.Content = buttonText;
                    button.Visibility = Visibility.Visible;
                });

            // Log the view event named "userViewedButton" to Dev Center.
            if (logger == null)
            {
                logger = StoreServicesCustomEventLogger.GetDefault();
            }

            logger.LogForVariation(variation, "userViewedButton");
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Log the conversion event named "userClickedButton" to Dev Center.
            if (logger == null)
            {
                logger = StoreServicesCustomEventLogger.GetDefault();
            }

            logger.LogForVariation(variation, "userClickedButton");
        }
     }
  }
  ```
10. Speichern Sie die Codedatei, und erstellen Sie das Projekt.

## Erstellen des Experiments in Windows Dev Center

1. Kehren Sie zur Projektseite **Button Click Experiments** im Windows Dev Center-Dashboard zurück.
2. Klicken Sie im Abschnitt **Experimente** auf die Schaltfläche **Neues Experiment**.
5. Geben Sie im Abschnitt **Experiment details** den Namen **Optimize Button Clicks** im Feld **Experiment name** ein.
6. Geben Sie im Abschnitt **View event** den Namen **userViewedButton** im Feld **Anzeige Ereignisname** ein. Dieser Name entspricht der Anzeigeereigniszeichenfolge, die Sie im Code protokolliert haben, der im vorherigen Abschnitt hinzugefügt wurde.
7. Geben Sie im Abschnitt **Goals and conversion events** die folgenden Werte ein:
  * Geben Sie im Feld **Goal name** **Increase Button Clicks** ein.
  * Geben Sie im Feld **Umwandlungsereignisname** den Namen **userClickedButton** ein. Dieser Name entspricht der Umwandlungsereigniszeichenfolge, die Sie im Code protokolliert haben, der im vorherigen Abschnitt hinzugefügt wurde.
  * Wählen Sie im Feld **Ziel** die Option **Maximieren**.
8. Stellen Sie im Abschnitt **Remote variables and variations** sicher, dass das Kontrollkästchen **Gleichmäßig verteilen** aktiviert ist, damit die Varianten gleichmäßig über Ihre App verteilt werden.
9. Fügen Sie dem Experiment Variablen hinzu:
  9. Klicken Sie auf das Dropdownsteuerelement, wählen Sie **buttonText** aus, und klicken Sie auf **Variable hinzufügen**. Die Zeichenfolge **Graue Schaltfläche** sollte automatisch in der Spalte **Variation A** angezeigt werden (dieser Wert wird von den Projekteinstellungen abgeleitet). Geben Sie in der Spalte **Variation B** den Text **Blaue Schaltfläche** ein.
  9. Klicken Sie erneut auf das Dropdownsteuerelement, wählen Sie **r** aus, und klicken Sie auf **Variable hinzufügen**. Die Zeichenfolge **128** sollte automatisch in der Spalte **Variation A** angezeigt werden. Geben Sie in der Spalte **Variation B** die Zahl **1** ein.
  9. Klicken Sie erneut auf das Dropdownsteuerelement, wählen Sie **g** aus, und klicken Sie auf **Variable hinzufügen**. Die Zeichenfolge **128** sollte automatisch in der Spalte **Variation A** angezeigt werden. Geben Sie in der Spalte **Variation B** die Zahl **1** ein.  
  9. Klicken Sie erneut auf das Dropdownsteuerelement, wählen Sie **b** aus, und klicken Sie auf **Variable hinzufügen**. Die Zeichenfolge **128** sollte automatisch in der Spalte **Variation A** angezeigt werden. Geben Sie in der Spalte **Variation B** die Zahl **255** ein.  
10. Klicken Sie auf **Speichern** und dann auf **Aktivieren**.

> **Wichtig**&nbsp;&nbsp;Nach dem Aktivieren eines Experiments können Sie die Experimentparameter nicht mehr ändern, wenn Sie beim Erstellen des Experiments nicht auf das Kontrollkästchen **Editable experiment** geklickt haben. In der Regel wird empfohlen, das Experiment vor der Aktivierung in der App zu codieren.

## Ausführen der App, um Experimentdaten zu sammeln

1. Führen Sie die App **SampleExperiment** aus, die Sie zuvor erstellt haben.
2. Stellen Sie sicher, dass entweder eine graue oder eine blaue Schaltfläche angezeigt wird. Klicken Sie auf die Schaltfläche, und schließen Sie dann die App.
3. Wiederholen Sie die obigen Schritte auf demselben Computer mehrmals, um zu bestätigen, dass in Ihrer App die gleiche Schaltflächenfarbe angezeigt wird.

## Überprüfen der Ergebnisse Abschließen des Experiments

Warten Sie nach Abschluss des vorherigen Abschnitts mindestens ein paar Stunden, und führen Sie dann diese Schritte aus, um die Ergebnisse Ihres Experiments zu überprüfen und das Experiment abzuschließen.

> **Hinweis**&nbsp;&nbsp;Sobald Sie ein Experiment aktivieren, beginnt DevCenter umgehend mit der Erfassung von Daten aus allen Apps, die zum Protokollieren von Daten für Ihr Experiment instrumentiert sind. Bis zur Anzeige von Experimentdaten im Dashboard können jedoch mehrere Stunden vergehen.

1. Kehren Sie in Dev Center zur Seite **Experimentation** für Ihre App zurück.
2. Klicken Sie im Abschnitt **Active experiments** auf **Optimize Button Clicks**, um zur Seite für dieses Experiment zu wechseln.
3. Vergewissern Sie sich, dass die Ergebnisse in den Abschnitten **Results summary** und **Results details** Ihren Erwartungen entsprechen. Ausführlichere Informationen zu diesen Abschnitten finden Sie unter [Verwalten des Experiments im Dev Center-Dashboard](manage-your-experiment.md#review-the-results-of-your-experiment).

  >**Hinweis**&nbsp;&nbsp;Dev Center meldet nur das erste Umwandlungsereignis für jeden Benutzer innerhalb eines Zeitraums von 24 Stunden. Wenn ein Benutzer innerhalb von 24 Stunden mehrere Umwandlungsereignisse in Ihrer App auslöst, wird nur das erste Umwandlungsereignis gemeldet. So soll verhindert werden, dass die Experimentergebnisse für eine Stichprobengruppe von Benutzern durch einen einzelnen Benutzer mit mehreren Umwandlungsereignissen verfälscht wird.
4. Jetzt sind Sie bereit, das Experiment zu beenden. Klicken Sie im Abschnitt **Results summary** in der Spalte **Variation B** auf **Wechseln**. Dadurch wird für alle Benutzer Ihrer App zur blauen Schaltfläche gewechselt.
5. Klicken Sie auf **OK**, um zu bestätigen, dass Sie das Experiment beenden möchten.
6. Führen Sie die App **SampleExperiment** aus, die Sie im vorhergehenden Abschnitt erstellt haben.
7. Vergewissern Sie sich, dass eine blaue Schaltfläche angezeigt wird. Beachten Sie, dass es bis zu zweiMinuten dauern kann, bis Ihre App eine aktualisierte Variantenzuweisung erhält.

## Verwandte Themen

* [Erstellen eines Projekts und Festlegen von Remotevariablen im Dev Center-Dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Programmieren Ihrer App für Experimente](code-your-experiment-in-your-app.md)
* [Definieren des Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
* [Verwalten des Experiments im Dev Center-Dashboard](manage-your-experiment.md)
* [Ausführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Sep16_HO1-->


