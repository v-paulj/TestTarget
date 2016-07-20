---
description: "Erfahren Sie, wie Sie eine UWP-App entwerfen und kodieren, die eine einfache Navigation besitzt und auf vielen Geräten und Bildschirmen verschiedener Größen großartig aussieht."
title: "Layoutdesign von UWP-Apps – Entwicklung von Windows-Apps"
author: mijacobs
keywords: uwp app layout, universal windows platform, app design, interface
translationtype: Human Translation
ms.sourcegitcommit: fa1567d3ff80dc9c9376736c7d25c2bb06e79cc9
ms.openlocfilehash: 7a1233724b9b0fd9af709bbf114669fe102e6f0d

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

<div class="uwpd-banner">
<h1 class="uwpd-ruledheader">Layout für UWP-Apps</h1>
</div>

App-Struktur, Seitenlayout und Navigation bilden die Grundlage der Benutzerumgebung Ihrer App. Die Artikel in diesem Abschnitt helfen Ihnen, eine App zu erstellen, in der Benutzer leicht navigieren können und die auf einer Vielzahl von Geräten und Bildschirmgrößen hervorragend aussieht.

## Einführung

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p><b>[Einführung in das UI-Design von Apps](design-and-ui-intro.md)</b><br />
Der Entwurf einer UWP-App umfasst auch die Erstellung einer Benutzeroberfläche, die für eine Vielzahl von Geräten mit unterschiedlichen Monitorgrößen geeignet ist. Dieser Artikel enthält eine Übersicht über UI-bezogene Features und Vorteile von UWP-Apps sowie Tipps und Tricks für das Entwerfen einer reaktionsfähigen Benutzeroberfläche. </p>
  </div>
  <div class="side-by-side-content-right">
    ![Eine App, die auf mehreren Geräten ausgeführt wird](images/rspd-reposition-type1-sm.png)
  </div>
</div>
</div>

## App-Layout und -Struktur
Sehen Sie sich diese Empfehlungen für das Strukturieren Ihrer App und das Verwenden der drei Arten von UI-Elementen an: Navigation, Befehle und Inhalte.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p>
<b>[Navigationsgrundlagen](navigation-basics.md)</b><br/>
Die Navigation in UWP-Apps basiert auf einem flexiblen Modell aus Navigationsstrukturen, Navigationselementen und Funktionen auf Systemebene. Dieser Artikel führt Sie in diese Komponenten ein und zeigt, wie Sie diese gemeinsam verwenden, um eine gute Navigationsumgebung zu erstellen.
</p>
<p>
<b>[Inhaltsgrundlagen](content-basics.md)</b><br/>
Der Hauptzweck jeder App ist das Bereitstellen von Zugriff auf Inhalte: In einer Fotobearbeitungs-App stellen Fotos den Inhalt dar, in einer Reise-App stellen Karten und Informationen über die Reiseziele den Inhalt dar, usw. Dieser Artikel stellt Empfehlungen für das Design von Inhalten für die drei Inhaltsszenarien bereit: Nutzung, Erstellung und Interaktion.
</p> 
  </div>
  <div class="side-by-side-content-right">
<p><b>[Befehlsgrundlagen](commanding-basics.md)</b> <br />
Befehlselementen sind die interaktiven UI-Elemente, mit denen der Benutzer Aktionen ausführen kann, um beispielsweise eine E-Mail zu senden, ein Element zu löschen oder ein Formular zu übermitteln. Dieser Artikel beschreibt Befehlselemente wie Schaltflächen und Kontrollkästchen, die von diesen unterstützten Interaktionen sowie die Befehlsoberflächen (wie Befehlsleisten und Kontextmenüs), auf denen sie sich befinden können.</p>
  </div>
</div>
</div>

## Seitenlayout 
Diese Artikel unterstützen Sie beim Erstellen einer flexiblen Benutzeroberfläche, die auch bei verschiedenen Bildschirmgrößen, Fenstergrößen, Auflösungen und Ausrichtungen hervorragend aussieht. 


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[Bildschirmgrößen und Haltepunkte](screen-sizes-and-breakpoints-for-responsive-design.md)</b><br/>
Die Anzahl der Geräteziele und Bildschirmgrößen bei Windows10 ist zu groß, um alle bei der Optimierung der Benutzeroberfläche zu berücksichtigen. Stattdessen wird empfohlen, für einige wichtige Bildschirmbreiten (auch als „Haltepunkte“ bezeichnet) zu entwickeln: 360, 640, 1024 und 1366 Epx.</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[Definieren von Layouts mit XAML](layouts-with-xaml.md)</b> <br/>
Hier wird gezeigt, wie Sie XAML-Eigenschaften und Layoutpanels verwenden, um eine reaktionsfähige und adaptive App zu entwickeln.</p>
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[Layoutpanels](layout-panels.md)</b> <br />
Erfahren Sie mehr über die einzelnen Arten von Layoutpaneln und wie diese für das Layout von XAML-UI-Elementen verwendet werden.</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[Ausrichtung, Ränder und Abstand](alignment-margin-padding.md)</b> <br />
Neben Abmessungseigenschaften (Breite, Höhe und Beschränkungen) können Elemente auch Ausrichtungs-, Rand- und Abstandseigenschaften aufweisen, die das Layoutverhalten beeinflussen, wenn ein Element einen Layoutdurchlauf ausführt und in einer Benutzeroberfläche dargestellt wird.</p> 
  </div>
</div>
</div>





<!--HONumber=Jul16_HO1-->


