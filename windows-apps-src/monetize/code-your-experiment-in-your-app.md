---
author: mcleanbyron
Description: "Zum Ausführen eines Experiments in Ihrer universellen Windows-Plattform (UWP)-App mit einem A/B-Test müssen Sie das Experiment in der App codieren."
title: "Programmieren der App für Experimente"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 29a94fd14d11256ade28463c04abfec81287cf39
ms.openlocfilehash: e5de32dcc7b0694e72d9686b3b9a64de17a02277

---

# Programmieren der App für Experimente

Nach dem [Erstellen eines Projekts und Festlegen von Remotevariablen im Dev Center-Dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md) können Sie den Code in Ihrer App für die universelle Windows-Plattform (UWP) aktualisieren, um:
* Werte von Remotevariablen von Windows Dev Center zu erhalten
* Remotevariablen zum Konfigurieren von App-Funktionen für die Benutzer zu verwenden
* Ereignisse in Dev Center zu protokollieren, die angeben, wann Benutzer Ihr Experiment angezeigt und eine gewünschte Aktion ausgeführt haben (auch als *Konvertierung* bezeichnet).

In den folgenden Abschnitten erfahren Sie ganz allgemein, wie Sie Variationen für Ihr Experiment erhalten und Ereignisse in Dev Center protokollieren. Nachdem Sie die App für Experimente programmiert haben, können Sie [ein Experiment im Dev Center-Dashboard definieren](define-your-experiment-in-the-dev-center-dashboard.md). Eine exemplarische Vorgehensweise, die den gesamten Erstellungs- und Ausführungsprozess für ein Experiment veranschaulicht, finden Sie unter [Erstellen und Durchführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

## Konfigurieren des Projekts

Installieren Sie zunächst das Microsoft Store Services SDK auf dem Entwicklungscomputer, und fügen Sie dem Projekt die erforderlichen Verweise hinzu.

1. Installieren Sie das [Microsoft Store Services SDK](http://aka.ms/store-em-sdk).
2. Öffnen Sie das Projekt in Visual Studio.
3. Erweitern Sie im Projektmappen-Explorer den Projektknoten, klicken Sie mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Verweis hinzufügen**.
3. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.
4. Aktivieren Sie in der Liste mit den SDKs das Kontrollkästchen neben **Microsoft Engagement Framework**, und klicken Sie anschließend auf **OK**.

## Hinzufügen von Code zum Abrufen von Variantendaten

Suchen Sie im Projekt nach dem Code für das Feature, das Sie in Ihrem Experiment ändern möchten. Fügen Sie Code hinzu, der Daten für eine Variante abruft, und verwenden Sie diese Daten, um das Verhalten des zu testenden Features zu ändern. Welchen Code Sie genau benötigen, hängt von Ihrer App ab. Hier werden daher nur die allgemeinen Schritte angegeben. Ein umfassendes Codebeispiel finden Sie unter [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Deklarieren Sie ein [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx)-Objekt, das die aktuelle Variantenzuweisung darstellt, und ein [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx)-Objekt zum Protokollieren von Anzeige- und Umwandlungsereignissen in Dev Center.
```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;
```

2. Rufen Sie die statische [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx)-Methode auf, um die aktuelle, zwischengespeicherte Variantenzuweisung für Ihr Experiment abzurufen, und übergeben Sie die [Projekt-ID](run-app-experiments-with-a-b-testing.md#terms) für das Experiment an die Methode. Diese Methode gibt ein [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx)-Objekt zurück, das den Zugriff auf die Variantenzuweisung (über die [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx)-Eigenschaft) ermöglicht.
  >**Hinweis**&nbsp;&nbsp;Sie erhalten eine Projekt-ID, wenn Sie [ein Projekt im Dev Center-Dashboard erstellen](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). Der hier gezeigte Projekt-ID dient nur als Beispiel.

  ```CSharp
var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(
      "F48AC670-4472-4387-AB7D-D65B095153FB");
variation = result.ExperimentVariation;
```

4. Überprüfen Sie anhand der [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx)-Eigenschaft, ob die zwischengespeicherte Variantenzuweisung mit einer Remotevariantenzuweisung vom Server aktualisiert werden muss. Falls ja, rufen Sie die statische [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx)-Methode auf, um auf dem Server nach einer aktualisierten Variantenzuweisung zu suchen und die lokal zwischengespeicherte Variante zu aktualisieren.
```CSharp
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
```

5. Verwenden Sie die [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean.aspx)-, [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble.aspx)-, [GetInt32](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32.aspx) oder [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx)-Methode des [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx)-Objekts, um die Werte für die Variantenzuweisung abzurufen. Der erste Parameter in den Methoden ist jeweils der Name der Variante, die Sie abrufen möchten (gemäß Eingabe auf dem Dev Center-Dashboard). Der zweite Parameter ist der Standardwert, den die Methode zurückgeben soll, falls der angegebene Wert nicht aus dem Dev Center abgerufen werden kann (etwa, weil keine Netzwerkverbindung besteht) und keine zwischengespeicherte Version der Variante verfügbar ist.

  Im folgenden Beispiel wird mithilfe von [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) eine Variable namens *buttonText* und der Standardvariablenwert **Grey Button** angegeben.
```CSharp
var buttonText = variation.GetString("buttonText", "Grey Button");
```
4. Verwenden Sie die Variablenwerte im Code, um das Verhalten des zu testenden Features zu ändern. Der folgende Code weist beispielsweise den *buttonText*-Wert dem Inhalt einer Schaltfläche zu.
```CSharp
button.Content = buttonText;
```

## Hinzufügen von Code zum Protokollieren von Anzeige- und Umwandlungsereignissen in Dev Center

Fügen Sie im nächsten Schritt Code hinzu, der die [Anzeigeereignisse](run-app-experiments-with-a-b-testing.md#terms) und [Umwandlungsereignisse](run-app-experiments-with-a-b-testing.md#terms) im A/B-Testdienst in Dev Center protokolliert. Der Code muss das Anzeigeereignis protokollieren, wenn der Benutzer damit beginnt, sich eine Variante anzusehen, die Teil Ihres Experiments ist. Das Umwandlungsereignis muss protokolliert werden, wenn der Benutzer ein Ziel Ihres Experiments erreicht.

Welchen Code Sie genau benötigen, hängt von Ihrer App ab. Hier werden daher nur die allgemeinen Schritte angegeben. Ein umfassendes Codebeispiel finden Sie unter [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Initialisieren Sie in dem Code, der ausgeführt wird, wenn der Benutzer mit der Betrachtung einer Variante beginnt, das ```logger```-Feld für ein [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx)-Objekt, und rufen Sie die [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx)-Methode auf. Übergeben Sie das [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx)-Objekt, das die aktuelle Variantenzuweisung darstellt (dieses Objekt liefert den Ereigniskontext für Dev Center), und den Namen des Anzeigeereignisses (gemäß Eingabe auf dem Dev Center-Dashboard). Im folgenden Beispiel wird ein Anzeigeereignis namens **userViewedButton** protokolliert.

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

2. Rufen Sie in dem Code, der ausgeführt wird, wenn der Benutzer eines der Ziele für das Experiment erreicht, erneut die [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx)-Methode auf, und übergeben Sie das [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx)-Objekt und den Namen eines Umwandlungsereignisses (gemäß Eingabe auf dem Dev Center-Dashboard). Im folgenden Beispiel wird ein Umwandlungsereignis namens **userClickedButton** protokolliert.
```CSharp
logger.LogForVariation(variation, "userClickedButton");
```

## Nächste Schritte

Nachdem Sie das Experiment in Ihrer App programmiert haben, sind Sie bereit für die folgenden Schritte:
1. [Definieren Sie das Experiment im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md). Erstellen Sie ein Experiment, das die Anzeigeereignisse, Umwandlungsereignisse und eindeutigen Varianten für den A/B-Test festlegt.
2. [Führen Sie das Experiment im Dev Center-Dashboard aus, und verwalten Sie es](manage-your-experiment.md).


## Verwandte Themen

* [Erstellen eines Projekts und Festlegen von Remotevariablen im Dev Center-Dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Definieren des Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
* [Verwalten des Experiments im Dev Center-Dashboard](manage-your-experiment.md)
* [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ausführen von App-Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Sep16_HO1-->


