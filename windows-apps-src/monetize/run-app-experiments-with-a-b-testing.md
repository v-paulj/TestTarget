---
author: mcleanbyron
Description: "Sie können über das Windows Dev Center-Dashboard Experimente für Ihre UWP-Apps (Universelle Windows-Plattform) mit A/B-Tests durchführen."
title: "Ausführen von Experimenten mit A/B-Tests"
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 50f7ad90c04d5b5672fa7910f2df669798761472

---

# Ausführen von Experimenten mit A/B-Tests

Sie können im Windows Dev Center-Dashboard Remote-Variablen definieren, die Sie zur Laufzeit von Ihren UWP-Apps abrufen können. Darüber hinaus haben Sie die Möglichkeit, Varianten dieser Werte mit ihren Benutzer zu testen, um die effektivsten Werte zu identifizieren und so das gewünschte Benutzerverhalten zu erhalten. Ihre App kann Remotevariablen verwenden, um App-Funktionen zu konfigurieren, z. B. In-App-Käufe, Anmeldungsfluss, Überschriften und Platzierung von Werbung.

Ziel des A/B-Tests sollte sein, eine Variante Ihrer Remotevariablenwerte zu identifizieren, die Ihnen wahrscheinlich bessere Konversionsraten einbringt (z.B. mehr In-App-Käufe), da eine interessantere App-Erfahrung bereitgestellt wird. Wenn Sie eine erfolgreiche Variante identifiziert haben, können Sie das Experiment sofort beenden und diese Variante über das Dev Center-Dashboard für die gesamte Zielgruppe aktivieren, ohne die App erneut veröffentlichen zu müssen.

## Erstellen und Ausführen eines A/B-Tests

Führen Sie zum Erstellen und Ausführen eines A/B-Tests die folgenden Schritte aus:

1. [Erstellen eines Projekts und Festlegen von Remotevariablen im Dev Center-Dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). Dieses Projekt enthält die Variablen und Standardvariablenwerte für Ihre Experimente.  
2. [Codieren der App für das Experiment](code-your-experiment-in-your-app.md). Verwenden Sie eine API im Microsoft Store Services SDK, um Remotevariablenwerte von dem Projekt abzurufen, das Sie im Dashboard erstellt haben. Ändern Sie mit diesen Daten das Verhalten des getesteten Features, und senden Sie das Anzeigeereignis und die Umwandlungsereignisse an Dev Center.
3. [Definieren Ihres Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md). Erstellen Sie ein Experiment in Ihrem Projekt, das die eindeutigen Ziele und Varianten für Ihren A/B-Test definiert.
4. [Ausführen und Verwalten Ihres Experiments im Dev Center-Dashboard](manage-your-experiment.md). Aktivieren Sie Ihr Experiment. Verwenden Sie das Dashboard, um die Ergebnisse des Experiments zu überprüfen und das Experiment abzuschließen.

Eine exemplarische Vorgehensweise, in der der Prozess vollständig dargestellt wird, finden Sie unter [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md) (Erstellen und Ausführen eines ersten Experiments mit A/B-Tests).

## Anforderungen

A/B-Tests im Windows Dev Center werden nur für UWP-Apps unterstützt.

Damit Sie Experimente mit A/B-Tests ausführen können, müssen Sie den Entwicklungscomputer einrichten:

* Führen Sie [diese Anweisungen](../get-started/get-set-up.md) aus, um den Entwicklungscomputer für die UWP-Entwicklung einzurichten.
* Installieren Sie das [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Neben der API für Experimente bietet dieses SDK auch APIs für andere Features, z.B. das Anzeigen von Werbung und das Weiterleiten von Kunden zum Feedback-Hub, um Feedback zur App zu erfassen. Weitere Informationen zu diesem SDK finden Sie unter [Microsoft Store Services SDK](microsoft-store-services-sdk.md).

## Bewährte Methoden

Um möglichst aussagekräftige Ergebnisse zu erzielen, sollten Sie beim Durchführen von Experimenten mit A/B-Tests diese Empfehlungen berücksichtigen:

* Führen Sie Experimente mit nur zwei Varianten mit einer zufälligen 50/50-Aufteilung für Variantenzuweisungen durch.
* Führen Sie die Experimente mindestens 2 bis 4Wochen lang durch, um ausreichend Daten zu erfassen, die statistisch signifikant und aussagekräftig sind.

<span id="terms" />
## Verwandte Begriffe

|  Begriff  |  Definition  |
|--------|--------------|
| Projekt    |   Eine Auflistung von Remotevariablen mit Standardwerten, auf die Ihre App mithilfe des Microsoft Store Services SDK zugreifen kann. Ein Projekt kann optional auch ein oder mehrere Experimente enthalten, die die gleichen Remotevariablen nutzen.  |
| Experiment    |   Ein Satz von Parametern, die einen A/B-Test definieren, den Ihre Benutzer erhalten. Experimente sind im Umfang eines Projekts definiert. Jedes Experiment besteht aus folgenden Elementen: <p></p><ul><li>Ein *Anzeigeereignis*, das angibt, wann der Benutzer mit dem Anzeigen einer Variante beginnt, die Teil des Experiments ist.</li><li>Eines oder mehrere Ziele mit *Umwandlungsereignissen*, um anzugeben, wann ein Ziel erreicht wurde.</li><li>Eine oder mehrere *Varianten*, die die im Experiment verwendeten Variablendaten definieren. Die *Steuerelement*-Variante verwendet die Standardvariablenwerte, die im Projekt für das Experiment definiert sind. Zusätzlich zu der Steuerungsvariation verfügen Experimente in der Regel über mindestens eine zusätzliche Variante mit Variablenwerten, die speziell für das Experiment gelten. </li></ul>          |
| Projekt-ID    |   Eine eindeutige ID, die Ihre App mit einem Projekt in Ihrem Dev Center-Konto verknüpft. Sie müssen diese ID verwenden, um eine Verbindung zu dem A/B-Testdienst in Ihrem App-Code herzustellen und so Variantendaten zu empfangen und Anzeige- und Umwandlungsereignisse an Dev Center zu melden. Weitere Informationen finden Sie unter [Programmieren Ihrer App für Experimente](code-your-experiment-in-your-app.md).<p></p><p>Jedes Projekt und alle Experimente im Projekt sind genau einer Projekt-ID zugeordnet. Sie können Projekt-IDs verwenden, um zwischen verschiedenen Gruppen von Experimenten zu unterscheiden. Beispielsweise haben Sie möglicherweise einen Satz von Experimenten, den Sie für Tester in Ihrer Organisation freigeben, und einen anderen Satz von Experimenten, den Sie nur für externe Benutzer Ihrer App freigeben.  Eine App kann auf mehrere Projekt-IDs verweisen, wenn sie mehrere Experimente implementiert.</p>         |
| Variante    |   Eine Sammlung von einer oder mehreren Variablen, die Sie in Ihrem Experiment testen. Jedes Experiment muss mindestens eine Variable und zwei Varianten umfassen (einschließlich des Steuerelements). Ein Experiment kann bis zu fünf Varianten aufweisen.           |
| Variable    |  Ein Wert, den Ihre App verwendet, um eine Eigenschaft oder einen anderen Wert in Ihrer App zu initialisieren. Während eines Experiments ändert sich der Wert der Variable von Variante zu Variante. Nachdem Sie das Experiment beendet haben, wird die Variable dem Wert der Variante zugewiesen, die Sie für die Freigabe für alle Benutzer Ihrer App auswählen. Variablen können die folgenden Typen aufweisen: eine Zeichenfolge, Boolean, Double und ganze Zahl.
| Anzeigeereignis    |  Eine beliebige Zeichenfolge, die eine Aktivität darstellt, wenn der Benutzer beginnt, eine Variante anzuzeigen, die Teil Ihres Experiments ist. In der Regel ist dies der Name eines Ereignisses in Ihrem Code. Der App-Code sendet die Zeichenfolge des Anzeigeereignisses an Dev Center, wenn der Benutzer beginnt, eine Variante anzuzeigen. Weitere Informationen finden Sie unter [Programmieren Ihrer App für Experimente](code-your-experiment-in-your-app.md).
| Umwandlungsereignis    |  Eine beliebige Zeichenfolge, die eine Zielsetzung für das Ziel eines Experiments darstellt. In der Regel ist dies der Name eines Ereignisses in Ihrem Code. Ihr App-Code sendet diese Umwandlungsereignis-Zeichenfolge an Dev Center, wenn der Benutzer ein Ziel erreicht. Weitere Informationen finden Sie unter [Programmieren Ihrer App für Experimente](code-your-experiment-in-your-app.md).  

## Verwandte Themen

* [Erstellen eines Projekts und Festlegen von Remotevariablen im Dev Center-Dashboard](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Programmieren Ihrer App für Experimente](code-your-experiment-in-your-app.md)
* [Definieren Ihres Experiments im Dev Center-Dashboard](define-your-experiment-in-the-dev-center-dashboard.md)
* [Verwalten Ihrer Experimente im Dev Center-Dashboard](manage-your-experiment.md)
* [Erstellen und Ausführen Ihres ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)



<!--HONumber=Aug16_HO3-->


