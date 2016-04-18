---
Description: Nachdem Sie Ihr Experiment im Dev Center-Dashboard definiert haben, können Sie es in Ihrer App programmieren.
title: Programmieren der App für Experimente
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
---

# Programmieren der App für Experimente

Nach dem [Definieren des Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md) können Sie den Code in Ihrer App für die universelle Windows-Plattform (UWP) aktualisieren, um Variantendaten für das Experiment zu erhalten, das Verhalten des getesteten Features anhand dieser Daten anzupassen sowie Anzeige- und Umwandlungsereignisse in Dev Center zu protokollieren.

In den folgenden Abschnitten erfahren Sie ganz allgemein, wie Sie Variationen für Ihr Experiment erhalten und Ereignisse in Dev Center protokollieren. Eine exemplarische Vorgehensweise, die den gesamten Erstellungs- und Ausführungsprozess für ein Experiment veranschaulicht, finden Sie unter [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

## Konfigurieren des Projekts

Installieren Sie zunächst das Microsoft Store Engagement and Monetization SDK auf dem Entwicklungscomputer, und fügen Sie dem Projekt die erforderlichen Verweise hinzu.

1. Installieren Sie das [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk).
2. Öffnen Sie das Projekt in Visual Studio.
3. Erweitern Sie im Projektmappen-Explorer den Projektknoten, klicken Sie mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Verweis hinzufügen**.
3. Erweitern Sie im Verweis-Manager**** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.
4. Aktivieren Sie in der Liste mit den SDKs das Kontrollkästchen neben **Microsoft Store Engagement SDK**, und klicken Sie anschließend auf **OK**.

## Hinzufügen von Code zum Abrufen von Varianteneinstellungen

Suchen Sie im Projekt nach dem Code für das Feature, das Sie in Ihrem Experiment ändern möchten. Fügen Sie Code hinzu, der Einstellungen für eine Variante abruft, und verwenden Sie diese Daten, um das Verhalten des zu testenden Features zu ändern. Welchen Code Sie genau benötigen, hängt von Ihrer App ab. Hier werden daher nur die allgemeinen Schritte angegeben. Ein umfassendes Codebeispiel finden Sie unter [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Deklarieren Sie ein [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx)-Objekt zum Abrufen von Varianten für Ihr Experiment und ein [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx)-Objekt, das die aktuelle Variantenzuweisung darstellt.
```
private readonly ExperimentClient experiment;
private ExperimentVariation variation;
```

2. Initialisieren Sie das [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx)-Objekt, und übergeben Sie den API-Schlüssel von der Dashboardseite **Experimente** an den Konstruktor. Weitere Informationen zum API-Schlüssel finden Sie unter [Definieren des Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md#generate-an-api-key). Der hier gezeigte API-Schlüssel dient nur als Beispiel.
```
experiment = new ExperimentClient("F48AC670-4472-4387-AB7D-D65B095153FB");
```

3. Rufen Sie die [GetVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.getvariationasync.aspx)-Methode auf, um die aktuelle, zwischengespeicherte Variantenzuweisung für Ihr Experiment abzurufen. Diese Methode gibt ein [ExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.aspx)-Objekt zurück, das den Zugriff auf die Variantenzuweisung (über die [Variation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.variation.aspx)-Eigenschaft) ermöglicht.
```
ExperimentVariationResult result = await experiment.GetVariationAsync();
variation = result.Variation;
```

4. Überprüfen Sie anhand der [NeedsRefresh](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.needsrefresh.aspx)-Eigenschaft, ob die zwischengespeicherte Variantenzuweisung aktualisiert werden muss. Falls ja, rufen Sie die [RefreshVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.refreshvariationasync.aspx)-Methode auf, um auf dem Server nach einer aktualisierten Variantenzuweisung zu suchen und die lokal zwischengespeicherte Variante zu aktualisieren.
```
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
{
      result = await experiment.RefreshVariationAsync();

      // If the call succeeds, use the new result. Otherwise, use the
      // cached value we retrieved earlier.
      if (result.ErrorCode == EngagementErrorCode.Success)
      {
          variation = result.Variation;
      }
}
```

5. Verwenden Sie die [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getboolean.aspx)-, [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getdouble.aspx)-, [GetInteger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getinteger.aspx)- oder [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx)-Methode des [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx)-Objekts, um die Einstellungen für die Variantenzuweisung abzurufen. Der erste Parameter in den Methoden ist jeweils der Name der Einstellung, die Sie abrufen möchten (gemäß Eingabe auf dem Dev Center-Dashboard). Der zweite Parameter ist der Standardwert, den die Methode zurückgeben soll, falls der angegebene Wert nicht aus dem Dev Center abgerufen werden kann (etwa, weil keine Netzwerkverbindung besteht) und keine zwischengespeicherte Version der Variante verfügbar ist.

  Im folgenden Beispiel wird mithilfe von [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) eine Einstellung namens *buttonText* abgerufen und der Standardeinstellungswert **Grey Button** angegeben.
```
var buttonText = currentVariation.GetString("buttonText", "Grey Button");
```
4. Verwenden Sie die Einstellungswerte im Code, um das Verhalten des zu testenden Features zu ändern. Der folgende Code weist beispielsweise den *buttonText*-Wert dem Inhalt einer Schaltfläche zu.
```
button.Content = buttonText;
```

## Hinzufügen von Code zum Protokollieren von Anzeige- und Umwandlungsereignissen in Dev Center

Fügen Sie im nächsten Schritt Code hinzu, der die Anzeige- und Umwandlungsereignisse im A/B-Testdienst in Dev Center protokolliert. Der Code muss das Anzeigeereignis protokollieren, wenn der Benutzer damit beginnt, sich eine Variante anzusehen, die Teil Ihres Experiments ist. Das Umwandlungsereignis muss protokolliert werden, wenn der Benutzer ein Ziel Ihres Experiments erreicht.

Welchen Code Sie genau benötigen, hängt von Ihrer App ab. Hier werden daher nur die allgemeinen Schritte angegeben. Ein umfassendes Codebeispiel finden Sie unter [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Rufen Sie in dem Code, der ausgeführt wird, wenn der Benutzer mit der Betrachtung einer Variante beginnt, die statische [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx)-Methode für das [StoreServicesCustomEvents](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.aspx)-Objekt auf. Übergeben Sie den Namen des Anzeigeereignisses, das Sie in Ihrem Experiment im Dev Center-Dashboard definiert haben, sowie das [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx)-Objekt, das die aktuelle Variantenzuweisung darstellt. (Dieses Objekt liefert den Ereigniskontext für Dev Center.) Im folgenden Beispiel wird ein Anzeigeereignis namens **userViewedButton** protokolliert.
```
StoreServicesCustomEvents.Log("userViewedButton", variation);
```
2. Rufen Sie in dem Code, der ausgeführt wird, wenn der Benutzer eines der Ziele für das Experiment erreicht, erneut die [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx)-Methode auf, und übergeben Sie den Namen eines in Ihrem Experiment definierten Umwandlungsereignisses sowie das [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx)-Objekt. Im folgenden Beispiel wird ein Umwandlungsereignis namens **userClickedButton** protokolliert.
```
StoreServicesCustomEvents.Log("userClickedButton", variation);
```

## Nächste Schritte

Nachdem Sie Ihr Experiment im Dev Center-Dashboard definiert und in Ihrer App programmiert haben, können Sie das [Experiment im Dev Center-Dashboard ausführen und verwalten](manage-your-experiment.md).

## Verwandte Themen

  * [Definieren des Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Verwalten des Experiments im Dev Center-Dashboard](manage-your-experiment.md)
  * [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Ausführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)


<!--HONumber=Mar16_HO5-->


