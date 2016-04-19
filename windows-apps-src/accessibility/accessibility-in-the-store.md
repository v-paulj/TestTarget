---
Description: Hier werden die Anforderungen beschrieben, die Sie erfüllen müssen, wenn Sie Ihre App für die universelle Windows-Plattform (UWP) im Windows Store als barrierefrei deklarieren möchten.
title: Barrierefreiheit im Store
ms.assetid: 59FA3B87-75A6-4B30-BA7C-A0E769D68050
label: Store requirements
template: detail.hbs
---

Barrierefreiheit im Store
=========================================================================================

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Hier werden die Anforderungen beschrieben, die Sie erfüllen müssen, wenn Sie Ihre UWP-App (Universelle Windows-Plattform) im Windows Store als barrierefrei deklarieren möchten.

Während Sie Ihre App zur Zertifizierung an den Windows Store übermitteln, können Sie sie als barrierefrei deklarieren. Indem Sie Ihre App als barrierefrei deklarieren, kann sie von Benutzern, die an barrierefreien Apps interessiert sind (z. B. Benutzer mit Sehschwächen), leichter gefunden werden. Benutzer entdecken barrierefreie Apps, indem sie beim Durchsuchen des Windows Store den Filter **Barrierefrei** verwenden. Wenn Sie Ihre App als barrierefrei deklarieren, wird der Beschreibung Ihrer App auch das Kennzeichen **Barrierefrei** hinzugefügt.

Indem Sie Ihre App als barrierefrei deklarieren, erklären Sie, dass sie über die [grundlegenden Informationen zur Barrierefreiheit](basic-accessibility-information.md) verfügt, die Benutzer für wichtige Szenarien unter Verwendung einer oder mehrerer der folgenden Dinge benötigen:

-   Die Tastatur.
-   Ein Design mit hohem Kontrast.
-   Eine variable dpi-Einstellung (dots per inch).
-   Allgemeine Hilfstechnologien wie z. B. die Windows-Eingabehilfefunktionen, darunter Sprachausgabe, Bildschirmlupe und Bildschirmtastatur.

Sie sollten Ihre App als barrierefrei erklären, wenn Sie diese unter Berücksichtigung der Barrierefreiheit erstellt und getestet haben. Dafür müssen Sie Folgendes gemacht haben:

-   Alle relevanten Barrierefreiheitsinfos für UI-Elemente sind festgelegt, einschließlich Name, Rolle, Wert usw.
-   Barrierefreiheit für den Tastaturzugriff implementieren. Benutzer haben so folgende Möglichkeiten:
    -   Sämtliche wichtigen App-Szenarien nur mithilfe der Tastatur auszuführen.
    -   UI-Elemente in einer logischen Reihenfolge durchzugehen.
    -   Mithilfe der Pfeiltasten zwischen den UI-Elementen eines Steuerelements zu navigieren.
    -   Tastenkombinationen für wichtige Funktionen der App zu verwenden.
    -   Verwenden von Sprachausgabe-Touchgesten für TAB- und Pfeilnavigation für Geräte ohne Tastatur.
-   Visuelle Barrierefreiheit der App-UI sicherstellen: Kontrastverhältnis von mindestens 4.5:1, keine ausschließlich auf Farben basierende Darstellung von Informationen usw.
-   Tools zum Testen der Barrierefreiheit wurden eingesetzt, z. B. [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) und [**UIAVerify**](https://msdn.microsoft.com/library/windows/desktop/Hh920986), um die Implementierung der Barrierefreiheit zu überprüfen und alle Fehler der Priorität 1 zu beheben, die von diesen Tools gemeldet werden.
-   Die primären Szenarien Ihrer App unter Verwendung von Sprachausgabe, Bildschirmlupe, Bildschirmtastatur, einem Design mit hohem Kontrast und angepassten DPI-Einstellungen wurden überprüft.

Unter [Prüfliste für die Barrierefreiheit](accessibility-checklist.md) werden diese Verfahren erläutert. Zudem finden Sie dort Links zu Ressourcen, die Ihnen beim Durchführen der Verfahren helfen.

<span id="related_topics"></span>Verwandte Themen
-----------------------------------------------

* [Barrierefreiheit](accessibility.md)
 

 





<!--HONumber=Mar16_HO3-->

