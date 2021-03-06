---
author: mijacobs
Description: "In diesem Artikel werden bewährte Methoden für das Erstellen und Anzeigen von App-Einstellungen beschrieben."
title: "Richtlinien für App-Einstellungen"
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 78ede41d559613e89d3174430f5474559f48c6bf
ms.openlocfilehash: 6302ec1bf332a27986876dbde4da92bbc916f85b

---


# Richtlinien für App-Einstellungen

App-Einstellungen sind die vom Benutzer anpassbaren Teile Ihrer App und live innerhalb der Einstellungsseite einer App. Beispielsweise kann ein Benutzer in einer Newsreader-App angeben, welche neuen Quellen oder wie viele Spalten auf dem Bildschirm angezeigt werden sollen. Und die Einstellungen einer Wetter-App können es dem Benutzer ermöglichen, zwischen Celsius und Fahrenheit als Standardmaßeinheit zu wählen. In diesem Artikel werden bewährte Methoden für das Erstellen und Anzeigen von App-Einstellungen beschrieben.

![Beispiel für einen Einstellungsbereich](images/app-settings.png)

## Sollte die App eine Seite für App-Einstellungen beinhalten?

Hier sind Beispiele für App-Optionen, die auf eine Seite für App-Einstellungen gehören: 

-   Konfigurationsoptionen, die sich auf das Verhalten der App auswirken und keine häufige erneute Anpassung erfordern (z.B. die Auswahl der Standardtemperatureinheit Celsius oder Fahrenheit in einer Wetter-App, das Ändern der Kontoeinstellungen für eine E-Mail-App, Einstellungen für Benachrichtigungen oder Barrierefreiheitsoptionen).
-   Optionen, die von den bevorzugten Benutzereinstellungen abhängen, wie Musik, Soundeffekte oder Farbdesigns.
-   App-Infos, die eher selten benötigt werden, z.B. Datenschutzrichtlinie, Hilfe, App-Version oder Copyright-Informationen.

Befehle, die Teil des typischen App-Workflows sind (z.B. das Ändern der Pinselgröße in einer Zeichen-App), gehören nicht auf eine Einstellungsseite. Weitere Informationen zur Platzierung von Befehlen finden Sie unter [Befehlsdesigngrundlagen](../layout/commanding-basics.md).

## Allgemeine Empfehlungen

-   Halten Sie die Einstellungsseiten möglichst einfach, und verwenden Sie binäre Steuerelemente (an/aus). Ein [Umschalter](../controls-and-patterns/toggles.md) ist in der Regel das beste Steuerelement für binäre Einstellungen.
-   Verwenden Sie [Optionsfelder](../controls-and-patterns/radio-button.md) für Einstellungen, mit denen Benutzer aus bis zu fünf zusammenhängenden Optionen, die sich gegenseitig ausschließen, eine auswählen können.
-   Erstellen Sie einen Einstiegspunkt für alle App-Einstellungen auf der Einstellungsseite Ihrer App.
-   Verwenden Sie einfache Einstellungen. Definieren Sie intelligente Standardwerte, und halten Sie die Anzahl von Einstellungen so gering wie möglich.
-   Wenn ein Benutzer eine Einstellung ändert, sollte die App die Änderung sofort wiedergeben.
-   Fügen Sie keine Befehle hinzu, die zu einem häufig verwendeten App-Workflow gehören.

## Einstiegspunkt


Die Methode, mit der Benutzer auf die Einstellungsseite Ihrer App zugreifen, sollte auf dem Layout Ihrer App basieren.

**Navigationsbereich**

Bei einem Navigationsbereichs-Layout sollten App-Einstellungen das letzte Element in der Navigationsliste und am Ende der Liste angeheftet sein:

![App-Einstellungen-Einstiegspunkt für Navigationsbereich](images/appsettings-entrypoint-navpane.png)

**App-Leiste**

Platzieren Sie bei Verwendung einer App-Leiste oder Symbolleiste, die in der Regel Teil des Navigationslayouts eines Hubs oder von Registerkarten/Pivots ist, den Einstiegspunkt als letztes Element im Flyoutmenü „Mehr“. Wenn der Einstiegspunkt für die Einstellungen in Ihrer App intuitiver positioniert werden soll, platzieren Sie diesen direkt auf der App-Leiste und nicht im Flyoutmenü „Mehr“.

![App-Einstellungen-Einstiegspunkt für die App-Leiste](images/appsettings-entrypoint-tabs.png)

**Hub**

Wenn Sie ein Hub-Layout verwenden, sollte der Einstiegspunkt für App-Einstellungen im Flyoutmenü „Mehr“ einer App-Leiste platziert werden.

**Registerkarten/Pivots**

Bei einem Registerkarten- oder Pivots-Layout raten wir davon ab, den Einstiegspunkt für App-Einstellungen als eines der Elemente der obersten Ebene in der Navigation zu platzieren. Stattdessen sollte der Einstiegspunkt für App-Einstellungen im Flyoutmenü „Mehr“ einer App-Leiste platziert werden.

**Master/Details**

Anstatt den Einstiegspunkt für App-Einstellungen innerhalb eines Master/Details-Bereichs zu u verstecken, machen Sie ihn als letztes angeheftete Element auf der obersten Ebene des Masterbereichs verfügbar.

## Layout


Auf Desktops und Mobilgeräten sollte sich das Fenster für die App-Einstellungen im Vollbildmodus öffnen und das gesamte Fenster ausfüllen. Wenn das App-Einstellungsmenü über bis zu vier Gruppen auf oberster Ebene verfügt, sollten diese Gruppen eine Spalte nach unten kaskadieren.

**Desktop**

![Layout der Seite für App-Einstellungen auf Desktops](images/appsettings-layout-navpane-desktop.png)

**Mobilgeräte**

![Layout der Seite für App-Einstellungen auf Smartphones](images/appsettings-layout-navpane-mobile.png)

## Abschnitt „Info“ und Schaltfläche „Feedback“


Wenn Sie einen Abschnitt „Info zu dieser App“ in Ihrer App benötigen, erstellen Sie dafür eine dedizierte App-Einstellungsseite. Platzieren Sie eine gewünschte Schaltfläche „Feedback“ im unteren Bereich der Seite „Info zu dieser App“.

„Nutzungsbedingungen“ und „Datenschutzbestimmungen“ sollten [Linkschaltflächen](../controls-and-patterns/hyperlinks.md) mit Textumbruch sein.

![Abschnitt „Info“ mit Schaltfläche „Feedback“](images/appsettings-about.png)

## Empfehlungen


### Inhalt der Seite für App-Einstellungen


Wenn Sie eine Liste der gewünschten Elemente auf der Seite für App-Einstellungen erstellt haben, berücksichtigen Sie die folgenden Richtlinien:

-   Gruppieren Sie ähnliche oder verwandte Einstellungen unter einer Einstellungsbezeichnung.
-   Versuchen Sie, die Gesamtanzahl der Einstellungen auf maximal vier oder fünf zu begrenzen.
-   Zeigen Sie unabhängig vom App-Kontext dieselben Einstellungen an. Sind einige Einstellungen in einem bestimmten Kontext nicht relevant, deaktivieren Sie sie im Flyoutmenü für die Einstellungen der App.
-   Verwenden Sie für Einstellungen informative Beschriftungen, die nur aus einem Wort bestehen. Nennen Sie für kontobezogene Einstellungen die Einstellung „Konten“ statt „Kontoeinstellungen“. Wenn Sie nur eine Option für Ihre Einstellungen festlegen möchten und die Einstellungen selbst sich nicht als Beschriftung eignen, verwenden Sie „Optionen“ oder „Standardeinstellungen“.
-   Wird über eine Einstellung direkt das Web anstelle eines Flyouts aufgerufen, informieren Sie den Benutzer mit einem als [Hyperlink](../controls-and-patterns/hyperlinks.md) formatierten visuellen Hinweis darüber, z.B. mit „Hilfe (online)“ oder „Webforen“. Mehrere Weblinks sollten Sie in einem Flyout mit einer einzelnen Einstellung gruppieren. Beispiel: Die Einstellung „Info“ könnte ein Flyout mit Links zu Ihren Nutzungsbedingungen, Datenschutzbestimmungen und App-Support-Infos öffnen.
-   Fassen Sie selten verwendete Einstellungen zu einem einzelnen Eintrag zusammen, damit für gängigere Einstellungen jeweils ein eigener Eintrag zur Verfügung steht. Fassen Sie Inhalte oder Links, die nur Informationen enthalten, unter der Einstellung „Info“ zusammen.
-   Wiederholen Sie die Funktionen nicht im Berechtigungsbereich. Windows stellt diesen Bereich standardmäßig bereit, und er kann nicht geändert werden.

###  Hinzufügen von Einstellungsinhalten zum Einstellungen-Flyout


-   Stellen Sie Inhalte von oben nach unten in einer einzelnen, ggf. bildlauffähigen Spalte dar. Beschränken Sie den Bildlauf auf die doppelte Bildschirmhöhe.
-   Verwenden Sie die folgenden Steuerelemente für App-Einstellungen:

    -   [Umschalter](../controls-and-patterns/toggles.md): Benutzer können Werte aktivieren oder deaktivieren.
    -   [Optionsfelder](../controls-and-patterns/radio-button.md): Benutzer können aus bis zu fünf zusammenhängenden, sich gegenseitig ausschließenden Optionen eine Option auswählen.
    -   [Texteingabefeld](../controls-and-patterns/text-block.md): Benutzer können hier Text eingeben. Wählen Sie die Art des Texteingabefelds passend zum Typ des vom Benutzer erhaltenen Texts aus, beispielsweise für E-Mail-Adressen oder Kennwörter.
    -   [Hyperlinks](../controls-and-patterns/hyperlinks.md): Benutzer werden zu einer anderen Seite innerhalb der App oder zu einer externen Website weitergeleitet. Wenn ein Benutzer auf einen Hyperlink klickt, wird das Einstellungen-Flyout geschlossen.
    -   [Schaltflächen](../controls-and-patterns/buttons.md): Benutzer können eine Aktion sofort ausführen, ohne das aktuell geöffnete Flyoutmenü für Einstellungen zu schließen.
-   Wenn eines der Steuerelemente deaktiviert ist, fügen Sie eine beschreibende Meldung hinzu. Platzieren Sie diese Meldung über dem deaktivierten Steuerelement.
-   Zeigen Sie Inhalte und Steuerelemente als einzelnen Block per Animation an, nachdem das Einstellungen-Flyout und die Überschrift eingeblendet wurden. Animieren Sie Inhalte mit der Animation [**enterPage**](https://msdn.microsoft.com/library/windows/apps/br212672) oder [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288) mit einem Offset links von 100Pixel.
-   Verwenden Sie Abschnittsüberschriften, Absätze und Bezeichnungen, um Inhalte ggf. zu organisieren und zu erläutern.
-   Verwenden Sie zum Wiederholen von Einstellungen eine zusätzliche UI-Ebene oder ein Model zum Erweitern und Reduzieren. Beschränken Sie Hierarchien jedoch auf maximal zweiEbenen. So könnte zum Beispiel in einer Wetter-App, deren Einstellungen sich auf die jeweilige Stadt beziehen, eine Liste mit Städten angezeigt werden. Der Benutzer muss dann nur auf die gewünschte Stadt zu tippen, um ein neues Flyout zu öffnen oder zu erweitern, um die Einstellungsoptionen anzuzeigen.
-   Wenn das Laden von Steuerelementen oder Webinhalten längere Zeit in Anspruch nimmt, informieren Sie den Benutzer mithilfe eines unbestimmten Statussteuerelements darüber, dass Informationen geladen werden. Weitere Informationen finden Sie unter [Richtlinien für Statussteuerelemente](../controls-and-patterns/progress-controls.md).
-   Verwenden Sie keine Schaltflächen für die Navigation oder zum Übernehmen von Änderungen. Verwenden Sie Hyperlinks, um zu anderen Seiten zu navigieren, und speichern Sie Änderungen an App-Einstellungen automatisch, wenn ein Benutzer das Einstellungen-Flyout schließt, anstatt eine Schaltfläche für das Übernehmen von Änderungen zu verwenden.

\[Dieser Artikel enthält spezielle Informationen zu Apps für die Universelle Windows-Plattform (UWP) und Windows 10. Wenn Sie Anleitungen für Windows8.1 benötigen, laden Sie die [PDF-Datei mit Windows8.1-Richtlinien](https://go.microsoft.com/fwlink/p/?linkid=258743) herunter.\]

## Verwandte Artikel

* [Befehlsdesigngrundlagen](../layout/commanding-basics.md)
* [Richtlinien für Statussteuerelemente](../controls-and-patterns/progress-controls.md)
* [Speichern und Abrufen von App-Daten](store-and-retrieve-app-data.md)
* [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288)




<!--HONumber=Aug16_HO3-->


