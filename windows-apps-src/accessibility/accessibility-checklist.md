---
Description: Enthält eine Prüfliste, mit der Sie sicherstellen können, dass Ihre App für die universelle Windows-Plattform (UWP) barrierefrei ist.
title: Prüfliste für die Barrierefreiheit
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
label: Checklist
template: detail.hbs
---

Prüfliste für die Barrierefreiheit
===================================================================================

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Enthält eine Prüfliste, mit der Sie sicherstellen können, dass Ihre UWP-App (Universelle Windows-Plattform) barrierefrei ist.

Hier finden Sie eine Prüfliste, die Sie verwenden können, um den Zugriff auf Ihre App sicherzustellen.

1.  Legen Sie den Namen (erforderlich) und die Beschreibung (optional) zur Verwendung durch Bildschirmleseprogramme für den Inhalt und die interaktiven UI-Elemente Ihrer App fest.

    Ein barrierefreier Name ist eine kurze, beschreibende Textzeichenfolge, mit der die Sprachausgabe ein UI-Element ansagt. Einige UI-Elemente wie [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) und [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) unterstützen ihren Textinhalt als standardmäßigen Namen für Bildschirmleseprogramme (siehe [Grundlegende Informationen zur Barrierefreiheit](basic-accessibility-information.md#name_from_inner_text)).

    Für Bilder oder andere Steuerelemente, bei denen der innere Text nicht als impliziter Name für Bildschirmleseprogramme verwendet werden kann, muss der Name explizit festgelegt werden. Verwenden Sie Bezeichnungen für Formularelemente, damit der Bezeichnungstext als [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769)-Ziel im Microsoft-Benutzeroberflächenautomatisierungs-Modell zum Korrelieren von Bezeichnungen und Eingaben verwendet werden kann. Wenn Sie mehr Informationen und Anweisungen zur Benutzeroberfläche für Benutzer bereitstellen möchten als normalerweise im Namen für Bildschirmleseprogramme enthalten sind, können Sie Beschreibungen und QuickInfos für Bildschirmleseprogramme implementieren.

    Weitere Informationen finden Sie unter [Name zur Verwendung durch Bildschirmleseprogramme](basic-accessibility-information.md#accessible_name) und [Beschreibung zur Verwendung durch Bildschirmleseprogramme](basic-accessibility-information.md).

2.  Implementieren Sie Barrierefreiheit für den Tastaturzugriff:


    -   Test the default tab index order for a UI. Adjust the tab index order if necessary, which may require enabling or disabling certain controls, or changing the default values of [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) on some of the UI elements.
    -   Use controls that support arrow-key navigation for composite elements. For default controls, the arrow-key navigation is typically already implemented.
    -   Use controls that support keyboard activation. For default controls, particularly those that support the UI Automation [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) pattern, keyboard activation is typically available; check the documentation for that control.
    -   Set access keys or implement accelerator keys for specific parts of the UI that support interaction.
    -   For any custom controls that you use in your UI, verify that you have implemented these controls with correct [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) support for activation, and defined overrides for key handling as needed to support activation, traversal and access or accelerator keys.

    For more info, see [Keyboard interactions](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3.  Schauen Sie sich die Benutzeroberfläche an, um sicherzustellen, dass der Textkontrast ausreicht, Elemente in Designs mit hohem Kontrast richtig dargestellt werden und Farben korrekt verwendet werden.

    -   Verwenden Sie die Systemanzeigeoptionen, die den DPI-Wert der Anzeige anpassen, und stellen Sie sicher, dass Ihre App-UI bei einer Änderung des DPI-Werts richtig skaliert wird. (Einige Benutzer ändern DPI-Werte als Barrierefreiheitsoption; diese ist unter **Erleichterte Bedienung** verfügbar.)
    -   Stellen Sie mithilfe eines Farbanalysetools sicher, dass das Textkontrastverhältnis mindestens 4,5:1 beträgt.
    -   Wechseln Sie zu einem Design mit hohem Kontrast, und überprüfen Sie, ob die Benutzeroberfläche Ihrer App leserlich ist und verwendet werden kann.
    -   Stellen Sie sicher, dass die Benutzeroberfläche Informationen nicht nur mithilfe von Farben vermittelt.

    Weitere Informationen finden Sie unter [Designs mit hohem Kontrast](high-contrast-themes.md) und [Anforderungen für barrierefreien Text](accessible-text-requirements.md).

4.  Führen Sie Tools zum Testen der Barrierefreiheit aus. Behandeln Sie gemeldete Probleme, und überprüfen Sie die Qualität der Sprachausgabe.

    Überprüfen Sie mithilfe von Tools wie [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) den programmgesteuerten Zugriff, führen Sie Diagnosetools wie [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) aus, um allgemeine Fehler zu ermitteln, und überprüfen Sie die Qualität der Sprachausgabe.

    Weitere Informationen finden Sie unter [Barrierefreiheitstests](accessibility-testing.md).

5.  Stellen Sie sicher, dass die App-Manifesteinstellungen den Richtlinien für Barrierefreiheit entsprechen.

6.  Deklarieren Sie Ihre App im Windows Store als barrierefrei.

    Wenn Sie die grundlegende Unterstützung für Barrierefreiheit implementiert haben, können Sie durch das Kennzeichnen Ihrer App im Windows Store mehr Kunden erreichen und zusätzliche gute Bewertungen erhalten.

    Weitere Informationen finden Sie unter [Barrierefreiheit im Store](accessibility-in-the-store.md).

<span id="related_topics"></span>Verwandte Themen
-----------------------------------------------

* [Barrierefreiheit](accessibility.md)
* [Entwerfen für Barrierefreiheit](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Nicht empfehlenswerte Methoden](practices-to-avoid.md)
 

 





<!--HONumber=Mar16_HO3-->


