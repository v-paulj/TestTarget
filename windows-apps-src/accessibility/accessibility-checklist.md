---
author: Xansky
Description: "Bietet eine Prüfliste, mit der Sie sicherstellen können, dass Ihre App für die Universelle Windows-Plattform (UWP) barrierefrei ist."
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: "Prüfliste für die Barrierefreiheit"
label: Accessibility checklist
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 6ac5430a3e18a02d5f2fc82829631b7649ea547d
ms.openlocfilehash: 2d632c524de8299378d8cc059d1c522080a2df2e

---

# Prüfliste für die Barrierefreiheit



Bietet eine Prüfliste, mit der Sie sicherstellen können, dass Ihre App für die Universelle Windows-Plattform (UWP) barrierefrei ist.

Hier finden Sie eine Prüfliste, die Sie verwenden können, um den Zugriff auf Ihre App sicherzustellen.

1.  Legen Sie den Namen (erforderlich) und die Beschreibung (optional) zur Verwendung durch Bildschirmleseprogramme für den Inhalt und die interaktiven UI-Elemente Ihrer App fest.

    Ein barrierefreier Name ist eine kurze, beschreibende Textzeichenfolge, mit der die Sprachausgabe ein UI-Element ansagt. Einige UI-Elemente wie [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) und [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) unterstützen ihren Textinhalt als standardmäßigen Namen für Bildschirmleseprogramme (siehe [Grundlegende Informationen zur Barrierefreiheit](basic-accessibility-information.md#name_from_inner_text)).

    Für Bilder oder andere Steuerelemente, bei denen der innere Text nicht als impliziter Name für Bildschirmleseprogramme verwendet werden kann, muss der Name explizit festgelegt werden. Verwenden Sie Bezeichnungen für Formularelemente, damit der Bezeichnungstext als [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769)-Ziel im Microsoft-Benutzeroberflächenautomatisierungs-Modell zum Korrelieren von Bezeichnungen und Eingaben verwendet werden kann. Wenn Sie mehr Informationen und Anweisungen zur Benutzeroberfläche für Benutzer bereitstellen möchten als normalerweise im Namen für Bildschirmleseprogramme enthalten sind, können Sie Beschreibungen und QuickInfos für Bildschirmleseprogramme implementieren.

    Weitere Informationen finden Sie unter [Name zur Verwendung durch Bildschirmleseprogramme](basic-accessibility-information.md#accessible_name) und [Beschreibung zur Verwendung durch Bildschirmleseprogramme](basic-accessibility-information.md).

2.  Implementieren Sie Barrierefreiheit für den Tastaturzugriff:

    * Testen Sie die standardmäßige Aktivierreihenfolge (Tabindex) für eine Benutzeroberfläche. Passen Sie die Aktivierreihenfolge ggf. an. Dazu müssen Sie möglicherweise bestimmte Steuerelemente aktivieren oder deaktivieren oder die Standardwerte von [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) für einige UI-Elemente ändern.
    * Verwenden Sie Steuerelemente, die eine Navigation mit Pfeiltasten für zusammengesetzte Elemente unterstützen. Für standardmäßige Steuerelemente ist die Navigation mit Pfeiltasten normalerweise bereits implementiert.
    * Verwenden Sie Steuerelemente, die die Tastaturaktivierung unterstützen. Für standardmäßige Steuerelemente (insbesondere diejenigen, die das [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582)-Muster der Benutzeroberflächenautomatisierung unterstützen) ist die Tastaturaktivierung normalerweise verfügbar. Hinweise dazu finden Sie in der Dokumentation der jeweiligen Steuerelemente.
    * Implementieren Sie Tastenkombinationen für bestimmte Teile der Benutzeroberfläche, die Interaktion unterstützen.
    * Überprüfen Sie für alle benutzerdefinierten Steuerelemente der Benutzeroberfläche, ob Sie sie mit der entsprechenden [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)-Unterstützung für die Aktivierung implementiert haben. Stellen Sie auch sicher, dass Sie die notwendigen Überschreibungen für die Tastenbehandlung definiert haben, um Aktivieren, Durchlaufen und Auswählen oder Tastenkombinationen zu unterstützen.

    Weitere Informationen finden Sie unter [Tastaturinteraktionen](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3.  Schauen Sie sich die Benutzeroberfläche an, um sicherzustellen, dass der Textkontrast ausreicht, Elemente in Designs mit hohem Kontrast richtig dargestellt werden und Farben korrekt verwendet werden.

    * Verwenden Sie die Systemanzeigeoptionen, die den DPI-Wert der Anzeige anpassen, und stellen Sie sicher, dass Ihre App-UI bei einer Änderung des DPI-Werts richtig skaliert wird. (Einige Benutzer ändern DPI-Werte als Barrierefreiheitsoption; diese ist unter **Erleichterte Bedienung** verfügbar.)
    * Stellen Sie mithilfe eines Farbanalysetools sicher, dass das Textkontrastverhältnis mindestens 4,5:1 beträgt.
    * Wechseln Sie zu einem Design mit hohem Kontrast, und überprüfen Sie, ob die Benutzeroberfläche Ihrer App leserlich ist und verwendet werden kann.
    * Stellen Sie sicher, dass die Benutzeroberfläche Informationen nicht nur mithilfe von Farben vermittelt.

    Weitere Informationen finden Sie unter [Designs mit hohem Kontrast](high-contrast-themes.md) und [Anforderungen für barrierefreien Text](accessible-text-requirements.md).

4.  Führen Sie Tools zum Testen der Barrierefreiheit aus. Behandeln Sie gemeldete Probleme, und überprüfen Sie die Qualität der Sprachausgabe.

    Überprüfen Sie mithilfe von Tools wie [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) den programmgesteuerten Zugriff, führen Sie Diagnosetools wie [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) aus, um allgemeine Fehler zu ermitteln, und überprüfen Sie die Qualität der Sprachausgabe.

    Weitere Informationen finden Sie unter [Barrierefreiheitstests](accessibility-testing.md).

5.  Stellen Sie sicher, dass die App-Manifesteinstellungen den Richtlinien für Barrierefreiheit entsprechen.

6.  Deklarieren Sie Ihre App im Windows Store als barrierefrei.

    Wenn Sie die grundlegende Unterstützung für Barrierefreiheit implementiert haben, können Sie durch das Kennzeichnen Ihrer App im Windows Store mehr Kunden erreichen und zusätzliche gute Bewertungen erhalten.

    Weitere Informationen finden Sie unter [Barrierefreiheit im Store](accessibility-in-the-store.md).

<span id="related_topics"/>
## Verwandte Themen  
* [Barrierefreiheit](accessibility.md)
* [Entwerfen für Barrierefreiheit](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Nicht empfehlenswerte Methoden](practices-to-avoid.md)



<!--HONumber=Jul16_HO2-->


