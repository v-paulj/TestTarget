---
author: Xansky
Description: "Testverfahren, mit denen Sie sicherstellen können, dass Ihre App für die universelle Windows-Plattform (UWP) barrierefrei ist."
ms.assetid: 272D9C9E-B179-4F5A-8493-926D007A0225
title: Barrierefreiheitstests
label: Accessibility testing
template: detail.hbs
ms.sourcegitcommit: 50c37d71d3455fc2417d70f04e08a9daff2e881e
ms.openlocfilehash: ec436f939c672d6e6d852d3dd6713fd6ca20a53b

---

# Barrierefreiheitstests  

Testverfahren, mit denen Sie sicherstellen können, dass Ihre App für die universelle Windows-Plattform (UWP) barrierefrei ist.

<span id="run_accessibility_testing_tools"/>
<span id="RUN_ACCESSIBILITY_TESTING_TOOLS"/>
## Ausführen der Tools zum Testen der Barrierefreiheit  
Das Windows Software Development Kit (SDK) enthält verschiedene Tools zum Testen der Barrierefreiheit, z. B. [**EH-Viewer**](https://msdn.microsoft.com/library/windows/desktop/Dn433239), [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) und [**UI-Barrierefreiheitsprüfung**](https://msdn.microsoft.com/library/windows/desktop/Hh920985). Mit diesen Tools können Sie die Barrierefreiheit Ihrer App überprüfen. Achten Sie darauf, dass Sie sämtliche App-Szenarien und UI-Elemente testen.

Sie können die Tools zum Testen der Barrierefreiheit entweder über die Eingabeaufforderung von Microsoft Visual Studio oder über den Tools-Ordner des Windows SDK öffnen (bin-Unterverzeichnis des Ordners, in dem das Windows SDK auf dem Entwicklungscomputer installiert ist).

<span id="AccScope"/>
<span id="accscope"/>
<span id="ACCSCOPE"/>
### **EH-Viewer (AccScope)**  

Mit dem Tool [**EH-Viewer**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) können Entwickler und Tester die Barrierefreiheit der App während ihrer Entwicklungs- und Entwurfsphase auswerten. Dies wird meist in frühen Prototypphasen und nicht in den letzten Testphasen des Entwicklungszyklus einer App durchgeführt. Das Hauptziel besteht im Testen von Barrierefreiheitsszenarien in Verbindung mit der Sprachausgabe für die App.

<span id="inspect"/>
<span id="INSPECT"/>
### **Inspect**  

[
              Im Tool **Inspect**
            ](https://msdn.microsoft.com/library/windows/desktop/Dd318521) können Sie ein beliebiges Benutzeroberflächenelement auswählen und Daten zu seiner Barrierefreiheit anzeigen. Sie können Eigenschaften und Steuerelementmuster der Microsoft-Benutzeroberflächenautomatisierung anzeigen und die Navigationsstruktur der Automatisierungselemente im Benutzeroberflächenautomatisierungs-Baum testen. Verwenden Sie beim Entwickeln der Benutzeroberfläche **Inspect**, um zu überprüfen, wie die Barrierefreiheitsattribute in der Benutzeroberflächenautomatisierung verfügbar gemacht werden. In einigen Fällen stammen die Attribute aus der Unterstützung der Benutzeroberflächenautomatisierung, die für Standard-XAML-Steuerelemente bereits implementiert wurde. In anderen Fällen stammen die Attribute aus bestimmten Werten, die Sie im XAML-Markup als an [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties) angefügte Eigenschaften festgelegt haben.

Die folgende Abbildung zeigt das [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521)-Tool, mit dem die Benutzeroberflächenautomatisierungseigenschaften des Menübefehls **Bearbeiten** in Editor abgefragt werden.

![Screenshot des Inspect-Tools.](./images/inspect.png)

<span id="ui_accessibility_checker"/>
<span id="UI_ACCESSIBILITY_CHECKER"/>
### **UI Accessibility Checker**  
Mithilfe der **UI-Barrierefreiheitsprüfung (AccChecker)** können Sie Probleme hinsichtlich der Barrierefreiheit zur Laufzeit erkennen. Wenn die Benutzeroberfläche vollständig und funktionsfähig ist, können Sie **AccChecker** verwenden, um verschiedene Szenarien zu testen, die Richtigkeit von Laufzeit-Barrierefreiheitsinformationen zu überprüfen und Laufzeitprobleme zu ermitteln. Sie können **AccChecker** im Benutzeroberflächen- oder Befehlszeilenmodus ausführen. Öffnen Sie zum Ausführen des Tools für den Benutzeroberflächenmodus das Verzeichnis **AccChecker** im bin-Verzeichnis des Windows SDK, führen Sie „acccheckui.exe“ aus, und klicken Sie auf das Menü **Hilfe**.

<span id="ui_automation_verify"/>
<span id="UI_AUTOMATION_VERIFY"/>
### **Benutzeroberflächenautomatisierungs-Überprüfung**  
Die **Benutzeroberflächenautomatisierungs-Überprüfung (UIA Verify)** ist ein automatisiertes Test- und Überprüfungsframework für Implementierungen der Benutzeroberflächenautomatisierung. **UIA Verify** kann in den Testcode integriert werden. Somit können regelmäßige automatisierte Tests oder Stichprobenkontrollen der Benutzeroberflächenautomatisierungs-Szenarien ausgeführt werden. Um **UIA Verify** auszuführen, führen Sie „VisualUIAVerifyNative.exe“ aus dem Unterverzeichnis „UIAVerify“ aus.

<span id="accessible_event_watcher"/>
<span id="ACCESSIBLE_EVENT_WATCHER"/>
### **Überwachung für barrierefreie Ereignisse**  
[
              Mit der **Überwachung für barrierefreie Ereignisse (AccEvent)**
            ](https://msdn.microsoft.com/library/windows/desktop/Dd317979) wird getestet, ob die UI-Elemente einer App korrekte Benutzeroberflächenautomatisierungs- und Microsoft Active Accessibility-Ereignisse auslösen, wenn Änderungen an der Benutzeroberfläche auftreten. Änderungen an der Benutzeroberfläche können auftreten, wenn sich der Fokus ändert, ein Benutzeroberflächenelement aufgerufen oder ausgewählt wird oder sich sein Zustand oder seine Eigenschaft ändert.

> [!NOTE]
> Die meisten in dieser Dokumentation erwähnten Tools zum Testen der Barrierefreiheit können auf einem PC, aber nicht auf einem Mobiltelefon ausgeführt werden. Sie können einige dieser Tools ausführen, während Sie entwickeln und einen Emulator verwenden, aber die meisten dieser Tools können den Benutzeroberflächenautomatisierungs-Baum im Emulator nicht bereitstellen.

<span id="test_keyboard_accessibility"/>
<span id="TEST_KEYBOARD_ACCESSIBILITY"/>
## Testen der Barrierefreiheit der Tastaturnavigation  
Die beste Methode, um die Barrierefreiheit der Tastaturnavigation zu testen, besteht darin, die Maus auszustöpseln oder im Fall eines Tabletgeräts die Bildschirmtastatur zu verwenden. Testen Sie die Barrierefreiheit der Tastaturnavigation mithilfe der TAB-Taste. Sie sollten alle interaktiven UI-Elemente mit der TAB-Taste erreichen können. Überprüfen Sie bei zusammengesetzten UI-Elementen, ob Sie mit den Pfeiltasten zwischen den einzelnen Elementteilen navigieren können. Sie sollten beispielsweise mithilfe der Tasten auf der Tastatur in Listen mit Elementen navigieren können. Stellen Sie zum Schluss sicher, dass Sie alle interaktiven UI-Elemente mithilfe der Tastatur aufrufen können, wenn der Fokus auf diesen Elementen liegt (in der Regel mit der EINGABETASTE oder LEERTASTE).

<span id="verify_the_contrast_ratio_of_visible_text"/>
<span id="VERIFY_THE_CONTRAST_RATIO_OF_VISIBLE_TEXT"/>
## Überprüfen des Kontrastverhältnisses von sichtbarem Text  
Überprüfen Sie mit den Farbkontrasttools, ob das Kontrastverhältnis von sichtbarem Text in Ordnung ist. Zu den Ausnahmen zählen inaktive UI-Elemente und Logos oder dekorativer Text, der keine Informationen enthält und neu angeordnet werden kann, ohne dadurch seine Bedeutung zu verändern. Weitere Informationen zum Kontrastverhältnis sowie zu Ausnahmen finden Sie unter [Anforderungen für barrierefreien Text](accessible-text-requirements.md). Tools zum Testen des Kontrastverhältnisses finden Sie unter [Techniken für WCAG 2.0 G18 (Abschnitt Ressourcen)](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

> [!NOTE]
> Einige der unter den Techniken für WCAG 2.0 G18 aufgeführten Tools können bei einer Windows Store-App nicht interaktiv verwendet werden. Möglicherweise müssen Sie die Farbwerte für den Vorder- und Hintergrund im Tool manuell eingeben, Bildschirmaufnahmen der App-Benutzeroberfläche anfertigen und dann das Kontrastverhältnistool für die Bildschirmaufnahme ausführen. Es kann auch erforderlich sein, dass Sie das Tool beim Öffnen der Quellbitmapdateien in einem Bildbearbeitungsprogramm ausführen, und nicht während des Ladens eines Bilds durch die App.

<span id="verify_your_app_in_high_contrast"/>
<span id="VERIFY_YOUR_APP_IN_HIGH_CONTRAST"/>
## Überprüfen der App mit hohem Kontrast  
Verwenden Sie die App mit einem Design mit hohem Kontrast, um sicherzustellen, dass alle UI-Elemente korrekt angezeigt werden. Der gesamte Text muss lesbar sein, und alle Bilder müssen deutlich zu erkennen sein. Passen Sie die XAML-Designverzeichnisressourcen oder Steuerelementvorlagen an, um durch Steuerelemente verursachte Designprobleme zu beheben. Wenn schwerwiegende Probleme mit hohem Kontrast nicht durch Designs oder Steuerelemente (z. B. durch Bilddateien) verursacht werden, stellen Sie separate Versionen bereit, die verwendet werden, wenn ein Design mit hohem Kontrast aktiv ist.

<span id="verify_your_app_with_make_everything_on_your_screen_bigger"/>
<span id="VERIFY_YOUR_APP_WITH_MAKE_EVERYTHING_ON_YOUR_SCREEN_BIGGER"/>
## Überprüfen der App mit Anzeigeeinstellungen  
Verwenden Sie die Systemanzeigeoptionen, die den DPI-Wert der Anzeige anpassen, und stellen Sie sicher, dass Ihre App-UI bei einer Änderung des DPI-Werts richtig skaliert wird. (Einige Benutzer ändern DPI-Werte als Barrierefreiheitsoption; sie ist wie auch Anzeigeeigenschaften unter **Erleichterte Bedienung** verfügbar.) Falls Sie Probleme feststellen, befolgen Sie die [Richtlinien zur Layoutskalierung](https://msdn.microsoft.com/library/windows/apps/Dn611863), und stellen Sie zusätzliche Ressourcen für unterschiedliche Skalierungsfaktoren bereit.

<span id="verify_main_app_scenarios_by_using_narrator"/>
<span id="VERIFY_MAIN_APP_SCENARIOS_BY_USING_NARRATOR"/>
## Überprüfen der wichtigsten App-Szenarien mit der Sprachausgabe  
Verwenden Sie die Sprachausgabe, um deren Qualität für Ihre App einschätzen zu können, indem Sie die folgenden Schritte durchführen:

**Gehen Sie wie folgt vor, um Ihre App mithilfe der Sprachausgabe unter Verwendung von Maus und Tastatur zu testen:**
1.  Starten Sie die Sprachausgabe, indem Sie Windows-Logo+EINGABE drücken.
2.  Bedienen Sie Ihre App mit der Tastatur mithilfe der TAB-TASTE, der PFEILTASTEN und der FESTSTELLTASTE+PFEILTASTEN.
3.  Hören Sie sich an, wie die Sprachausgabe Elemente der Benutzeroberfläche vorliest, während Sie die App bedienen, und achten Sie auf Folgendes:
    * Achten Sie bei allen Steuerelementen darauf, dass die Sprachausgabe korrekt für alle sichtbaren Inhalte erfolgt. Stellen Sie zudem sicher, dass die Sprachausgabe jeweils den Namen, alle relevanten Zustände (aktiviert, ausgewählt usw.) und den Typ des Steuerelements (Schaltfläche, Kontrollkästchen, Listenelement usw.) vorliest.
    * Überprüfen Sie bei einem interaktiven Element, ob Sie dessen Aktion per Sprachausgabe aufrufen können, indem Sie FESTSTELL+LEER drücken.
    * Achten Sie bei Tabellen darauf, dass die Sprachausgabe den Tabellennamen, die Tabellenbeschreibung (falls verfügbar) und die Zeilen- und Spaltenüberschriften richtig vorliest.

4.  Drücken Sie FESTSTELLTASTE+EINGABETASTE, um Ihre App zu durchsuchen und zu überprüfen, ob alle Steuerelemente in der Suchliste erscheinen und ob die Namen der Steuerelemente lokalisiert und lesbar sind.
5.  Schalten Sie den Monitor aus und versuchen Sie, die Hauptszenarien für Ihre App durchzuspielen, indem Sie nur Tastatur und Maus verwenden. Eine umfassende Liste aller Befehle und Tastenkombinationen erhalten Sie, indem Sie FESTSTELLTASTE+F1 drücken.

**Gehen Sie wie folgt vor, um Ihre App mithilfe des Toucheingabemodus der Sprachausgabe zu testen:**

> [!NOTE]
> Die Sprachausgabe wechselt auf Geräten, die mehr als 4 Kontakte unterstützen, automatisch in den Toucheingabemodus. Die Sprachausgabe unterstützt keine Szenarien mit mehreren Monitoren oder Digitalisierungsgeräte mit Multitouch auf dem Hauptbildschirm.

1.  Machen Sie sich mit der Benutzeroberfläche vertraut und nehmen Sie das Layout in Augenschein.

    * **Navigieren Sie per Wischbewegung mit einem Finger durch die Benutzeroberfläche.** Wischen Sie nach links oder rechts, um zwischen Elementen hin und her zu wechseln, und nach oben und unten, um die Kategorie der zu durchsuchenden Elemente zu ändern. Zu den Kategorien gehören alle Elemente, Links, Tabellen, Überschriften usw. Das Navigieren per Wischbewegung mit einem Finger gestaltet sich so ähnlich wie das Navigieren mithilfe von FESTSTELLTASTE+PFEILTASTE.
    * **Navigieren Sie mit Tippbewegungen durch Elemente, die den Fokus erhalten können.** Wenn Sie mit drei Fingern eine Wischbewegung nach rechts oder links ausführen, hat dies denselben Effekt wie das Drücken von TAB und UMSCHALT+TAB auf einer Tastatur.
    * **Überprüfen Sie die Benutzeroberfläche flächendeckend mit einem Finger.** Führen Sie einen Finger nach oben und unten oder links und rechts, um sich von der Sprachausgabe die Elemente unter Ihrem Finger vorlesen zu lassen. Alternativ können Sie auch eine Maus dazu nutzen, da bei ihr dieselbe Zugriffstestlogik zum Einsatz kommt wie beim Ziehen Ihres Fingers.
    * **Lassen Sie sich das ganze Fenster und alle Inhalte vorlesen, indem Sie mit drei Fingern eine Wischbewegung nach oben durchführen**. Diese Bewegung entspricht der Tastenkombination FESTSTELLTASTE+W.

    Sollten wichtige Elemente der UI nicht erreichbar sein, liegt vielleicht ein Fehler bei der Barrierefreiheit vor.

2.  Interagieren Sie mit Steuerelementen, um deren primäre und sekundäre Aktionen und Bildlaufverhalten zu testen.

    Primäre Aktionen sind zum Beispiel die Aktivierung einer Schaltfläche, das Einfügen eines Caretzeichens und das Legen des Fokus auf das Steuerelement. Unter sekundäre Aktionen fallen z. B. Aktionen wie die Auswahl eines Listenelements oder das Erweitern einer Schaltfläche, die mehrere Optionen anbietet.

    * So testen Sie eine primäre Aktion: Doppeltippen oder mit einem Finger drücken und mit einem anderen tippen.
    * So testen Sie eine sekundäre Aktion: Dreimal Tippen oder mit einem Finger drücken und mit einem anderen doppeltippen.
    * So testen Sie das Bildlaufverhalten: Mit zwei Fingern eine Wischbewegung in die gewünschte Richtung durchführen.

    Einige Steuerelemente bieten zusätzliche Aktionen. Rufen Sie eine vollständige Liste auf, indem Sie mit vier Fingern einmal tippen.

    Sollte ein Steuerelement auf Maus- und Tastatureingaben, jedoch nicht auf eine primäre oder sekundäre Berührungsinteraktion reagieren, müssen für das Steuerelement unter Umständen zusätzliche Steuerungsmuster zur [Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee684009) implementiert werden.

Erwägen Sie auch die Nutzung des [**EH-Viewer**](https://msdn.microsoft.com/library/windows/desktop/Dn433239)-Tools (EH-Viewer) zum Testen von Barrierefreiheitsszenarien in Verbindung mit der Sprachausgabe für die App. Im [**Thema zum EH-Viewer-Tool**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) wird erläutert, wie **EH-Viewer** für das Testen von Szenarien in Verbindung mit der Sprachausgabe konfiguriert wird.

<span id="Examine_the_UI_Automation_representation_for_your_app"/>
<span id="examine_the_ui_automation_representation_for_your_app"/>
<span id="EXAMINE_THE_UI_AUTOMATION_REPRESENTATION_FOR_YOUR_APP"/>
## Untersuchen der Darstellung der Benutzeroberflächenautomatisierung für Ihre App  
Einige der bereits erwähnten Tools zum Testen der Benutzeroberflächenautomatisierung ermöglichen eine Anzeige Ihrer App, bei der absichtlich nicht berücksichtigt wird, wie die App aussieht. Stattdessen wird die App als Struktur aus Benutzeroberflächenautomatisierungs-Elementen dargestellt. Mit diesem Verfahren interagieren Benutzeroberflächenautomatisierungs-Clients, vor allem Hilfstechnologien, mit der App in Barrierefreiheitsszenarien.

Das Tool [**EH-Viewer**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) ermöglicht eine besonders interessante Anzeige der App, da Sie die Benutzeroberflächenautomatisierungs-Elemente entweder als visuelle Darstellung oder als Liste anzeigen können. Wenn Sie die Visualisierung verwenden, können Sie einen Drilldown zu den einzelnen Bestandteilen so durchführen, dass eine Korrelation mit der visuellen Darstellung der App-UI möglich ist. Sie können sogar die Barrierefreiheit Ihrer frühesten UI-Prototypen testen, bevor Sie der UI die gesamte Logik zugewiesen haben. So stellen Sie sicher, dass sich die visuelle Interaktion und die Navigation für Barrierefreiheitsszenarien der App im Gleichgewicht befinden.

Außerdem können Sie testen, ob in der Elementansicht der Benutzeroberflächenautomatisierung Elemente angezeigt werden, die dort nicht erscheinen sollen. Falls Sie Elemente finden, die aus der Ansicht entfernt werden sollen, oder falls Elemente fehlen, können Sie mithilfe der angefügten XAML-Eigenschaft [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.accessibilityview) anpassen, wie XAML-Steuerelemente in Barrierefreiheitsansichten erscheinen. Nachdem Sie sich die grundlegenden Barrierefreiheitsansichten angesehen haben, ist dies ein guter Zeitpunkt für die erneute Überprüfung der Registerkartensequenzen oder der räumlichen Navigation mithilfe von Pfeiltasten. So können Sie sich vergewissern, dass Benutzer Zugang zu allen Teilbereichen haben, die interaktiv sind und in der Steuerungsansicht verfügbar gemacht werden.

<span id="related_topics"/>
## Verwandte Themen  
* [Barrierefreiheit](accessibility.md)
* [Nicht empfehlenswerte Methoden](practices-to-avoid.md)
* [Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee684009)
* [Barrierefreiheit unter Windows](http://go.microsoft.com/fwlink/p/?LinkId=320802)



<!--HONumber=Jun16_HO5-->


