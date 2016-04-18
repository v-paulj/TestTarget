---
Description: In dieser exemplarischen Vorgehensweise wird Ihr erstes Experiment mit A/B-Tests erstellt und ausgeführt.
title: Erstellen und Ausführen eines ersten Experiments mit A/B-Tests
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
---

# Erstellen und Ausführen eines ersten Experiments mit A/B-Tests

In dieser exemplarischen Vorgehensweise führen Sie folgende Aktionen aus:
* Erstellen eines Experiments im Windows Dev Center-Dashboard, das überprüft, ob die Anzahl von Klicks auf eine App-Schaltfläche durch das Ändern der Hintergrundfarbe der Schaltfläche erfolgreich erhöht werden kann
* Erstellen einer App, die Varianteneinstellungen aus Dev Center abruft, anhand dieser Daten die Hintergrundfarbe einer Schaltfläche ändert und Anzeige- sowie Umwandlungsereignisdaten in Dev Center protokolliert
* Ausführen der App, um Experimentdaten zu sammeln
* Überprüfen der Experimentergebnisse im Dev Center-Dashboard, Auswählen einer Variante zur Aktivierung für alle Benutzer der App und Abschließen des Experiments

Eine Übersicht über A/B-Tests mit Dev Center finden Sie unter [Ausführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md).

## Voraussetzungen

Zum Abschließen dieser exemplarischen Vorgehensweise benötigen Sie ein Windows Dev Center-Konto und müssen Ihren Entwicklungscomputer gemäß der Beschreibung in [Ausführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md) konfigurieren.

## Erstellen des Experiments Windows Dev Center

1. Melden Sie sich beim [Dev Center-Dashboard](https://dev.windows.com/overview) an.
2. Wenn Sie eine bereits in Dev Center vorhandene App zu Erstellen eines Experiments verwenden möchten, wählen Sie die App unter **Your Apps** aus. Wenn Ihr Dashboard noch keine App enthält, [erstellen Sie eine neue App durch Reservierung eines Namens](../publish/create-your-app-by-reserving-a-name.md) und wählen diese App dann im Dashboard aus.
3. Klicken Sie im Navigationsbereich auf **Dienste** und dann auf **Experimentation**.
4. Wählen Sie im Abschnitt **API-Schlüssel** die Option **New API key**, um einen neuen API-Schlüssel zu generieren, und geben den Namen **My First Experiment** für den API-Schlüssel ein. Diesen API-Schlüssel verwenden Sie im nächsten Abschnitt dieser exemplarischen Vorgehensweise.
5. Klicken Sie im Abschnitt **Experimente** auf **New experiment**. Geben Sie im Feld **Experiment name** den Namen **Optimize Button Clicks** ein.
6. Geben Sie im Feld **Anzeige Ereignisname** den Namen **UserViewedButton** ein. Später in dieser exemplarischen Vorgehensweise fügen Sie Code hinzu, der dieses Anzeigeereignis protokolliert, wenn die Hauptseite der App initialisiert wird und die Schaltfläche für den Benutzer sichtbar ist.
7. Geben Sie im Abschnitt **Goals and conversion events** die folgenden Werte ein:
  * Geben Sie im Feld **Goal name** **Increase Button Clicks** ein.
  * Geben Sie im Feld **Umwandlungsereignisname** den Namen **userClickedButton** ein. Später in dieser exemplarischen Vorgehensweise fügen Sie Code hinzu, der dieses Umwandlungsereignis im Click-Ereignishandler der Schaltfläche protokolliert.
  * Wählen Sie im Feld **Ziel** die Option **Maximieren**.
8. Klicken Sie im Abschnitt **Variations and settings** drei Mal auf **Einstellung hinzufügen**. Sie sollten jetzt über vier Zeilen mit leeren Einstellungen verfügen.
  * Geben Sie in der ersten Zeile **buttonText** als Einstellungsname ein, in der Spalte **Variation A** **Graue Schaltfläche** und in der Spalte **Variation B** **Blaue Schaltfläche**.
  * Geben Sie in der zweiten Zeile **r** als Einstellungsname ein, in der Spalte **Variation A** den Wert **128** und in der Spalte **Variation B** den Wert **1**.
  * Geben Sie in der dritten Zeile **g** als Einstellungsname ein, in der Spalte **Variation A** den Wert **128** und in der Spalte **Variation B** den Wert **1**.
  * Geben Sie in der vierten Zeile **b** als Einstellungsname ein, in der Spalte **Variation A** den Wert **128** und in der Spalte **Variation B** den Wert **255**.  
9. Stellen Sie sicher, dass das Kontrollkästchen **Gleichmäßig verteilen** aktiviert ist, damit die Varianten gleichmäßig über Ihre App verteilt werden.
10. Klicken Sie auf **Speichern** und dann auf **Aktivieren**.

> **Wichtig** Nach der Aktivierung eines Experiments können Sie die Experimentparameter nicht mehr ändern, sofern es sich nicht um ein Testexperiment handelt (d. h. Sie haben beim Erstellen des Experiments das Kontrollkästchen **Test experiment** aktiviert). In der Regel wird empfohlen, das Experiment vor der Aktivierung in der App zu codieren. Der Einfachheit halber können Sie in dieser exemplarischen Vorgehensweise das Experimentieren jetzt aktivieren.

## Codieren des Experiments in der App

1. Erstellen Sie in Visual Studio 2015 ein neues UWP-Projekt (Universelle Windows-Plattform) mit Visual C#. Geben Sie dem Projekt den Namen **SampleExperiment**.
2. Erweitern Sie im Projektmappen-Explorer den Projektknoten, klicken Sie mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Verweis hinzufügen**.
3. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App** und klicken Sie auf **Erweiterungen**.
4. Aktivieren Sie in der Liste mit den SDKs das Kontrollkästchen neben **Microsoft Store Engagement SDK**, und klicken Sie anschließend auf **OK**.
5. Doppelklicken Sie im **Projektmappen-Explorer** auf „MainPage.xaml“, um den Designer für die Hauptseite in der App zu öffnen.
6. Ziehen Sie eine **Schaltfläche** aus der **Toolbox** auf die Seite.
7. Doppelklicken Sie im Designer auf die Schaltfläche, um die Codedatei zu öffnen, und fügen Sie einen Ereignishandler für das **Click**-Ereignis hinzu.  
8. Ersetzen Sie den gesamten Inhalt der Codedatei mit folgendem Code.

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
        private readonly ExperimentClient experiment;
        private ExperimentVariation variation;

        // Assign this variable to your API key from Dev Center. The API key
        // shown below is for example purposes only.
        private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";    

        public MainPage()
        {
            this.InitializeComponent();

            // Initialize the ExperimentClient for A/B testing.
            experiment = new ExperimentClient(apiKey);

            // Because this call is not awaited, execution of the current method
            // continues before the call is completed.
            #pragma warning disable CS4014
            Initialize();
            #pragma warning restore CS4014
        }

        private async Task Initialize()
        {
            // Get the current cached variation assignment for the experiment.
            ExperimentVariationResult result = await experiment.GetVariationAsync();
            variation = result.Variation;

            // Check whether the cached variation assignment needs to be refreshed.
            // If so, then refresh it.
            if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
            {
                result = await experiment.RefreshVariationAsync();

                // If the call succeeds, use the new result. Otherwise, use the cached value.
                if (result.ErrorCode == EngagementErrorCode.Success)
                {
                    variation = result.Variation;
                }
            }

            // Get settings named "buttonText", "r", "g", and "b" from the variation
            // assignment. If no variation assignment is available, the settings default
            // to "Grey button" for the button text and grey RGB value for the button color.
            var buttonText = variation.GetString("buttonText", "Grey Button");
            var r = (byte)variation.GetInteger("r", 128);
            var g = (byte)variation.GetInteger("g", 128);
            var b = (byte)variation.GetInteger("b", 128);

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
            StoreServicesCustomEvents.Log("userViewedButton", variation);
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Log the conversion event named "userClickedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userClickedButton", variation);
        }
     }
  }
  ```
9. Weisen Sie in der folgenden Codezeile die *apiKey*-Variable dem API-Schlüssel zu, den Sie im vorhergehenden Abschnitt aus dem Dev Center-Dashboard abgerufen haben. Der hier gezeigte API-Schlüssel dient nur als Beispiel.
```CSharp
private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";
```
10. Speichern Sie die Codedatei, und erstellen Sie das Projekt.

## Ausführen der App, um Experimentdaten zu sammeln

1. Führen Sie die App **SampleExperiment** aus, die Sie im vorhergehenden Abschnitt erstellt haben.
2. Stellen Sie sicher, dass entweder eine graue oder eine blaue Schaltfläche angezeigt wird. Klicken Sie auf die Schaltfläche, und schließen Sie dann die App.
3. Wiederholen Sie die obigen Schritte auf demselben Computer mehrmals, um zu bestätigen, dass in Ihrer App die gleiche Schaltflächenfarbe angezeigt wird.

## Überprüfen der Ergebnisse Abschließen des Experiments

Warten Sie nach Abschluss des vorherigen Abschnitts mindestens ein paar Stunden, und führen Sie dann diese Schritte aus, um die Ergebnisse Ihres Experiments zu überprüfen und das Experiment abzuschließen.

> **Hinweis** Sobald Sie ein Experiment aktivieren, beginnt Dev Center umgehend mit der Erfassung von Daten aus allen Apps, die zum Protokollieren von Daten für Ihr Experiment instrumentiert sind. Bis zur Anzeige von Experimentdaten im Dashboard können jedoch mehrere Stunden vergehen.

1. Kehren Sie in Dev Center zur Seite **Experimentation** für Ihre App zurück.
2. Klicken Sie im Abschnitt **Experimente** auf den Filter **Aktiv** und dann auf **Optimize Button Clicks**, um zur Seite für dieses Experiments zu wechseln.
3. Vergewissern Sie sich, dass die Ergebnisse in den Abschnitten **Results summary** und **Results details** Ihren Erwartungen entsprechen. Ausführlichere Informationen zu diesen Abschnitten finden Sie unter [Verwalten des Experiments im Dev Center-Dashboard](manage-your-experiment.md#review-the-results-of-your-experiment).

  >**Hinweis** Dev Center meldet nur das erste Umwandlungsereignis für jeden Benutzer innerhalb eines Zeitraums von 24 Stunden. Wenn ein Benutzer innerhalb von 24 Stunden mehrere Umwandlungsereignisse in Ihrer App auslöst, wird nur das erste Umwandlungsereignis gemeldet. So soll verhindert werden, dass die Experimentergebnisse für eine Stichprobengruppe von Benutzern durch einen einzelnen Benutzer mit mehreren Umwandlungsereignissen verfälscht wird.

4. Jetzt sind Sie bereit, das Experiment zu beenden. Klicken Sie im Abschnitt **Results summary** in der Spalte **Variation B** auf **Wechseln**. Dadurch wird für alle Benutzer Ihrer App zur blauen Schaltfläche gewechselt.
5. Klicken Sie auf **OK**, um zu bestätigen, dass Sie das Experiment beenden möchten.
6. Führen Sie die App **SampleExperiment** aus, die Sie im vorhergehenden Abschnitt erstellt haben.
7. Vergewissern Sie sich, dass eine blaue Schaltfläche angezeigt wird. Beachten Sie, dass bis zu zwei Minuten dauern kann, bis Ihre App eine aktualisierte Variantenzuweisung erhält.

## Verwandte Themen

  * [Definieren des Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Programmieren der App für Experimente](code-your-experiment-in-your-app.md)
  * [Verwalten des Experiments im Dev Center-Dashboard](manage-your-experiment.md)
  * [Ausführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)


<!--HONumber=Mar16_HO5-->


