---
description: "Passen Sie Ihre UWP-App für bestimmte Arten von Eingaben und Geräten an."
title: "Eingabe und Gerätedesign – Entwicklung von Windows-Apps"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: fa1567d3ff80dc9c9376736c7d25c2bb06e79cc9
ms.openlocfilehash: f2055318fe67a0af2bcc009c6f9782029f9032f1

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

# Eingabe und Geräte

UWP-Apps verarbeiten automatisch eine Vielzahl von Eingaben und können auf zahlreichen Geräten ausgeführt werden. Sie müssen daher keine zusätzlichen Schritte zum Aktivieren der Toucheingabe oder zum Ausführen der App auf einem Smartphone ausführen. 

Unter Umständen möchten Sie jedoch Ihre App für bestimmte Eingabe- oder Gerätearten optimieren. Beim Erstellen einer Zeichen-App möchten Sie vielleicht die Behandlung der Stifteingabe anpassen. 

Die Design- und Codierungsanweisungen in diesem Abschnitt unterstützen Sie beim Anpassen Ihrer UWP-App für bestimmte Eingabe- und Gerätearten. 

## Eingaben und Interaktionen

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Einführung in Eingaben](input-primer.md)</b><br/> Machen Sie sich mit den verschiedenen Arten von Eingabegeräten sowie ihren Verhaltensweisen, Möglichkeiten und Einschränkungen in Verbindung mit bestimmten Formfaktoren vertraut.   
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Cortana](cortana-interactions.md) </b><br/> Erweitern Sie die Grundfunktionen von Cortana durch Sprachbefehle, die eine einzelne Aktion in einer externen Anwendung starten und ausführen.   
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>[Gamepad und Remotesteuerung](gamepad-and-remote-interactions.md)</b><br/>UWP-Apps unterstützen jetzt Gamepad- und Remotesteuerungseingaben. Gamepads und Remotesteuerungen sind die primären Eingabegeräte in Xbox- und TV-Umgebungen.  
  </div>
  <div class="side-by-side-content-right">
<b>[Tastatur](keyboard-interactions.md)</b><br/>Die Tastatureingabe ist ein wichtiger Teil der Erfahrung, die Benutzer bei der Interaktion mit Apps machen. Die Tastatur ist unentbehrlich für Personen mit bestimmten körperlichen Beeinträchtigungen oder für Benutzer, die die Tastatur als effizienteste Methode ansehen, um mit einer App zu interagieren.  
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Maus](mouse-interactions.md)</b><br/>Die Mauseingabe eignet sich am besten für Benutzerinteraktionen, die Präzision beim Zeigen und Klicken erfordern. Naturgemäß unterstützt die Benutzeroberfläche von Windows diese Präzision, auch wenn sie für die ungenaue Toucheingabe optimiert wurde.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Stift](pen-and-stylus-interactions.md)</b><br/>Optimieren Sie Ihre UWP-App für Stifteingaben, um Benutzern sowohl die Standardfunktionalität für Zeigegeräte als auch die optimale Windows Ink-Funktionalität bereitzustellen.   
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Spracherkennung](speech-interactions.md)</b><br/>Integrieren Sie Spracherkennung und Text-zu-Sprache (auch als Text-to-Speech, TTS, oder Sprachsynthese bezeichnet) direkt in die Benutzeroberfläche Ihrer App.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Toucheingabe](touch-interactions.md)</b><br/>Die UWP stellt eine Reihe unterschiedlicher Verfahren für die Behandlung von Toucheingaben bereit, mit denen Sie eine immersive Umgebung erstellen können, in der Ihre Benutzer zuverlässig navigieren können.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Touchpad](touchpad-interactions.md)  </b><br/>Gestalten Sie Ihre App so, dass Benutzer über ein Touchpad mit ihr interagieren können. Ein Touchpad vereint die indirekte Multitoucheingabe mit der Präzisionseingabe eines Zeigergeräts (beispielsweise eine Maus). Durch diese Kombination ist das Touchpad sowohl für eine toucheingabeoptimierte Benutzeroberfläche als auch für die kleineren Ziele von Produktivitäts-Apps geeignet.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Mehrfacheingaben](multiple-input-design-guidelines.md)  </b><br/>Um eine möglichst große Zahl von Benutzern und Geräten zu unterstützen, empfehlen wir, Ihre App für die Kompatibilität mit so vielen Eingabearten wie möglich zu entwickeln(Gesten, Spracherkennung, Toucheingabe, Touchpad, Maus und Tastatur). Dadurch werden Flexibilität, Benutzerfreundlichkeit und Barrierefreiheit optimiert.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Optischer Zoom und Größenänderung](guidelines-for-optical-zoom.md)</b><br/>In diesem Artikel werden die Windows-Elemente für das Zoomen und die Größenänderung beschrieben. Außerdem enthält das Thema Richtlinien für die Benutzeroberfläche, um diese Interaktionsmechanismen in Ihren Apps zu verwenden.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Verschieben](guidelines-for-panning.md)</b><br/>Mit einer Verschiebung oder einem Bildlauf können Benutzer innerhalb einer einzelnen Ansicht navigieren, um den Inhalt der Ansicht anzuzeigen, der nicht in den Anzeigebereich passt.  
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Drehung](guidelines-for-rotation.md)</b><br/> In diesem Artikel wird die neue Windows-Benutzeroberfläche beschrieben, die Drehungen unterstützt. Außerdem enthält das Thema Richtlinien für die Benutzeroberfläche, die Sie berücksichtigen sollten, wenn Sie diesen neuen Interaktionsmechanismus in einer UWP-App verwenden.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Auswählen von Text und Bildern](guidelines-for-textselection.md)</b><br/>In diesem Artikel wird das Auswählen und Bearbeiten von Text, Bildern und Steuerelementen beschrieben. Außerdem enthält er Richtlinien für die Benutzeroberfläche, die Sie bei Verwendung dieser Mechanismen in Ihren Apps berücksichtigen sollten.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Zielbestimmung](guidelines-for-targeting.md)</b><br/>Die Touchzielbestimmung in Windows verwendet den vollständigen Kontaktbereich jedes Fingers, der durch ein Touchdigitalisierungsgerät erkannt wird. Die größere und komplexere Menge an Eingabedaten, die vom Digitalisierungsgerät gemeldet wird, wird verwendet, um die Präzision bei der Ermittlung des durch den Benutzer (höchstwahrscheinlich) beabsichtigten Ziels zu erhöhen.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Visuelles Feedback](guidelines-for-visualfeedback.md)</b><br/>Zeigen Sie Benutzern durch visuelles Feedback, wenn ihre Interaktionen ermittelt, interpretiert und behandelt werden. Visuelles Feedback ist hilfreich für Benutzer und kann sie zur Interaktion ermutigen. Es weist auf erfolgreiche Interaktionen hin, was für den Benutzer das Gefühl der Kontrolle verstärkt. Darüber hinaus informiert es über den Systemstatus und verringert die Fehlerzahl.  
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Identifizieren von Eingabegeräten](identify-input-devices.md)</b><br/>Identifizieren Sie die Eingabegeräte, die mit einem Gerät für die universelle Windows-Plattform (UWP) verbunden sind, sowie deren Funktionen und Attribute. 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Benutzerdefinierte Texteingabe](custom-text-input.md)</b><br/>Mit den Core-Text-APIs im Windows.UI.Text.Core-Namespace kann eine UWP-App Texteingaben von allen Textdiensten empfangen, die auf Windows-Geräten unterstützt werden. Auf diese Weise kann die App Texte in einer beliebigen Sprache und von einem beliebigen Eingabegerät empfangen, wie Tastatur, Spracherkennung oder Stift.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Behandeln von Zeigereingaben](handle-pointer-input.md)</b><br/>Empfangen, verarbeiten und verwalten Sie Eingabedaten von Zeigegeräten wie Toucheingabe, Maus, Stift und Touchpad in Apps für die universelle Windows-Plattform (UWP).
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b><br/>   
</p>
  </div>
</div>
</div>


## Geräte

Wenn Sie sich mit den Geräten vertraut machen, die UWP-Apps unterstützen, können Sie für jeden Formfaktor eine optimale Benutzerumgebung bereitstellen. Beim Entwickeln für ein bestimmtes Gerät sind die wichtigsten zu berücksichtigenden Punkte die Darstellung der App auf dem Gerät; wo, wann und wie die App auf dem Gerät genutzt wird; und die Art der Interaktion der Benutzer mit dem Gerät.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Einführung in Geräte](device-primer.md)</b><br/>Wenn Sie sich mit den Geräten vertraut machen, die UWP-Apps unterstützen, können Sie für jeden Formfaktor eine optimale Benutzerumgebung bereitstellen. 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Entwerfen für Xbox und Fernsehgeräte](designing-for-tv.md)</b><br/>Entwerfen Sie Ihre App für die Universelle Windows-Plattform (UWP) so, dass sie gut aussieht und auf Xbox One- und Fernsehbildschirmen optimal funktioniert.
</p>
  </div>
</div>
</div>




<!--HONumber=Jul16_HO1-->


