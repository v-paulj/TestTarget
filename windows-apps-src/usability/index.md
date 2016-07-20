---
description: "Erfahren Sie, wie Sie Ihre App inklusiv entwickeln und für Personen auf der ganzen Welt zugänglich machen."
keywords: uwp app accessibility, globalization, design inclusive apps, accessibility app requirements
title: "Benutzerfreundlichkeit in UWP-Apps – Entwicklung von Windows-Apps"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: 5370dd078db81de606b7a4bbfcf6dd36b2280a17
ms.openlocfilehash: 457ddacd80b3f9308b2eb66ce0ac2d448bb59ed2

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

# Benutzerfreundlichkeit in UWP-Apps

Es sind die Kleinigkeiten und Details, die eine gute Benutzerumgebung zu einer wirklich inklusiven Benutzerumgebung machen, die die Anforderungen von Benutzern auf der ganzen Welt erfüllt.

Mit den Design- und Codierungsanweisungen in diesem Abschnitt können Sie Ihre UWP-App inklusiver gestalten, indem Sie Features für Barrierefreiheit hinzufügen, Globalisierung und Lokalisierung ermöglichen, das Anpassen der Benutzeroberfläche durch die Benutzer zulassen und im Bedarfsfall Hilfe für Benutzer bereitstellen.


## Barrierefreiheit

Bei der Barrierefreiheit geht es darum, die App so zu gestalten, dass sie auch von Menschen verwendet werden kann, bei denen bestimmte Beeinträchtigungen die Nutzung herkömmlicher Benutzeroberflächen verhindern oder erschweren. Für bestimmte Situationen gelten gesetzlich vorgeschriebene Vorgaben im Hinblick auf Barrierefreiheit. Es wird jedoch empfohlen, die Barrierefreiheit unabhängig von gesetzlichen Anforderungen zu berücksichtigen, um sicherzustellen, dass Ihre Apps die größtmögliche Benutzergruppe erreichen.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Übersicht über die Barrierefreiheit](../accessibility/accessibility-overview.md)</b> <br/> Dieser Artikel bietet eine Übersicht über die Konzepte und Technologien im Zusammenhang mit Barrierefreiheitsszenarien für UWP-Apps.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Entwerfen von inklusiver Software](../accessibility/designing-inclusive-software.md)</b><br/>Erfahren Sie mehr über das Entwerfen inklusiver Apps für die Universelle Windows-Plattform (UWP) für Windows 10.  Entwerfen und erstellen Sie inklusive Software unter Berücksichtigung der Barrierefreiheit.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Entwickeln von inklusiven Windows-Apps](../accessibility/developing-inclusive-windows-apps.md)</b><br/> Dieser Artikel dient als Orientierungshilfe bei der Entwicklung barrierefreier UWP-Apps.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Barrierefreiheitstests](../accessibility/accessibility-testing.md) </b><br/>Testverfahren, mit denen Sie sicherstellen können, dass Ihre UWP-App barrierefrei ist.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Barrierefreiheit im Store](../accessibility/accessibility-in-the-store.md)</b><br/>Hier werden die Anforderungen beschrieben, die Sie erfüllen müssen, wenn Sie Ihre UWP-App im WindowsStore als barrierefrei deklarieren möchten.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Prüfliste für die Barrierefreiheit](../accessibility/accessibility-checklist.md)</b><br/>Enthält eine Prüfliste, mit der Sie sicherstellen können, dass Ihre UWP-App barrierefrei ist.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Verfügbarmachen von grundlegenden Informationen zur Barrierefreiheit](../accessibility/basic-accessibility-information.md)</b><br/>Grundlegende Informationen zur Barrierefreiheit werden häufig in die Kategorien Name, Rolle und Wert unterteilt. In diesem Thema wird der Code beschrieben, mit dem Ihre App die grundlegenden Informationen verfügbar machen kann, die für Hilfstechnologien erforderlich sind.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Barrierefreiheit der Tastaturnavigation](../accessibility/keyboard-accessibility.md)</b><br/>Wenn Ihre App keine barrierefreie Bedienung mit der Tastatur ermöglicht, können Benutzer, die blind oder in ihrer Beweglichkeit eingeschränkt sind, Schwierigkeiten bei der Verwendung Ihrer App haben oder Ihre App möglicherweise überhaupt nicht nutzen.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Designs mit hohem Kontrast](../accessibility/high-contrast-themes.md)</b><br/>Beschreibt die Schritte, mit denen Sie die Bedienbarkeit Ihrer UWP-App sicherstellen können, wenn ein Design mit hohem Kontrast aktiviert ist. </p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Anforderungen für barrierefreien Text](../accessibility/accessible-text-requirements.md)</b><br/>In diesem Thema werden die bewährten Methoden für barrierefreien Text in Apps beschrieben. Damit stellen Sie sicher, dass der Kontrast zwischen Farben und Hintergründen ausreichend hoch ist. Außerdem werden in diesem Thema die Rollen der Microsoft-Benutzeroberflächenautomatisierung, die für Textelemente in einer UWP-App verwendet werden können, sowie bewährte Methoden für Text in Grafiken erläutert.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Nicht empfehlenswerte Praktiken für die Barrierefreiheit](../accessibility/practices-to-avoid.md)</b><br/>Es werden die Praktiken aufgeführt, die Sie vermeiden sollten, wenn Sie eine barrierefreie UWP-App erstellen möchten.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Benutzerdefinierte Automatisierungspeers](../accessibility/custom-automation-peers.md)</b><br/>Beschreibt das Konzept von Automatisierungspeers für die Benutzeroberflächenautomatisierung und erläutert, wie Sie Unterstützung zur Benutzeroberflächenautomatisierung für eigene benutzerdefinierte UI-Klassen bereitstellen können.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Steuerelementmuster und Schnittstellen](../accessibility/control-patterns-and-interfaces.md)</b><br/>Enthält eine Liste der Steuerelementmuster für die Microsoft-Benutzeroberflächenautomatisierung zusammen mit den Klassen, die Clients für den Zugriff verwenden, und den Schnittstellen, die Anbieter zur Implementierung verwenden.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b>   
</p>
  </div>
</div>
</div>



## Globalisierung und Lokalisierung

Windows wird auf der ganzen Welt von Benutzern mit unterschiedlicher Kultur, Herkunft und Sprache verwendet. Benutzer sprechen beliebige Sprache oder sogar mehrere Sprachen. Die Benutzer sind über die ganze Welt verteilt, und jede Sprache kann ortsabhängig überall gesprochen werden. Sie können das Marktpotenzial Ihrer App erweitern, indem Sie eine mithilfe von Globalisierung und Lokalisierung leicht anpassbare App entwickeln. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Empfohlene und nicht empfohlene Vorgehensweisen](../globalizing/guidelines-and-checklist-for-globalizing-your-app.md)</b><br/>Befolgen Sie diese bewährten Methoden beim Globalisieren Ihrer Apps für eine größere Zielgruppe und beim Lokalisieren Ihrer Apps für einen bestimmten Markt.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Verwenden weltweit einsetzbarer Formate](../globalizing/use-global-ready-formats.md)</b><br/>Entwickeln Sie eine für den globalen Markt geeignete App mit geeigneter Formatierung für Datumsangaben, Uhrzeiten, Zahlen und Währungen.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Verwalten von Sprache und Region](../globalizing/manage-language-and-region.md)</b><br/>Mithilfe der verschiedenen Sprach- und Regionseinstellungen von Windows können Sie die Auswahl von UI-Ressourcen und die Formatierung der UI-Elemente der App durch Windows steuern.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Verwenden von Mustern zum Formatieren von Datums- und Uhrzeitwerten](../globalizing/use-patterns-to-format-dates-and-times.md)</b><br/>Verwenden Sie die [<strong>DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859)-API mit benutzerdefinierten Mustern, um Datums- und Uhrzeitwerte im gewünschten Format anzuzeigen.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)</b><br/>Entwickeln Sie Ihre App so, dass die Layouts und Schriftarten mehrerer Sprachen unterstützt werden – also beispielsweise auch die Flussrichtung von rechts nach links (right-to-left, RTL).</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Vorbereiten Ihrer App für die Lokalisierung](../globalizing/prepare-your-app-for-localization.md)</b><br/>Bereiten Sie Ihre App für die Lokalisierung vor, um sie an andere Märkte, Sprachen oder Regionen anzupassen.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Aufnehmen von UI-Zeichenfolgen in Ressourcen](../globalizing/put-ui-strings-into-resources.md)</b><br/>Nehmen Sie Zeichenfolgenressourcen für Ihre Benutzeroberfläche in Ressourcendateien auf. Sie können dann in Ihrem Code oder Markup auf diese Zeichenfolgen verweisen.</p>
  </div>
  <div class="side-by-side-content-right">
<b></b>   
<p></p>
  </div>
</div>
</div>


## App-Einstellungen

Mithilfe von App-Einstellungen kann der Benutzer Ihre App anpassen und entsprechend den eigenen Anforderungen und Wünschen optimieren. Die Bereitstellung der richtigen Einstellungen und ihr ordnungsgemäßes Speichern können eine großartige Benutzerumgebung sogar noch weiter verbessern. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Anleitungen](../app-settings/guidelines-for-app-settings.md)</b><br/>Bewährte Methoden für das Erstellen und Anzeigen von App-Einstellungen.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Speichern und Abrufen von App-Daten](../app-settings/store-and-retrieve-app-data.md)</b><br/>Hier erfahren Sie, wie Sie lokale, Roaming- und temporäre App-Daten speichern und abrufen können.</p>
  </div>
</div>
</div>

## In-App-Hilfe
Unabhängig davon, wie optimal Sie Ihre App gestaltet haben, werden einige Benutzer etwas zusätzliche Hilfe benötigen. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Anleitungen für die App-Hilfe](../app-help-guidelines/guidelines-for-app-help.md)</b><br/>Anwendungen können sehr komplex sein. Sie können die Erfahrung für die Benutzer durch die Bereitstellung einer effektiven Hilfe erheblich verbessern. 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Benutzeroberfläche mit Anleitungen](../app-help-guidelines/instructional-ui.md)</b><br/>Manchmal kann es hilfreich sein, Anleitungen bereitzustellen, um Benutzern Funktionen in Ihrer App zu zeigen, die möglicherweise nicht offensichtlich sind, z.B. bestimmte Touchinteraktionen. In diesen Fällen müssen Sie Anleitungen auf der Benutzeroberfläche für die Benutzer bereitstellen, damit sie diese Funktionen, die sie möglicherweise übersehen haben, finden und nutzen können.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[In-App-Hilfe](../app-help-guidelines/in-app-help.md)</b><br/>In den meisten Fällen empfiehlt es sich, dass die Hilfe innerhalb der App angezeigt wird, wenn der Benutzer sie anzeigen möchte. Beachten Sie beim Erstellen der In-App-Hilfe die folgenden Anleitungen.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Externe Hilfe](../app-help-guidelines/external-help.md)</b><br/>In den meisten Fällen empfiehlt es sich, dass die Hilfe innerhalb der App angezeigt wird, wenn der Benutzer sie anzeigen möchte. Beachten Sie beim Erstellen der In-App-Hilfe die folgenden Anleitungen.</p>
  </div>
</div>
</div>






<!--HONumber=Jul16_HO1-->


