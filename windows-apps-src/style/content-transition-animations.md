---
Description: Mithilfe von Inhaltsübergangsanimationen können Sie den Inhalt eines Bildschirmbereichs ändern und gleichzeitig den Container oder Hintergrund unverändert lassen. Neuer Inhalt wird eingeblendet. Muss vorhandener Inhalt ersetzt werden, wird dieser Inhalt ausgeblendet.
title: Richtlinien für Inhaltsübergangsanimationen
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
---

# Inhaltsübergangsanimationen


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**Wichtige APIs**

-   [**ContentThemeTransition-Klasse (XAML)**](https://msdn.microsoft.com/library/windows/apps/br243104)
-   [**enterContent-Funktion (HTML)**](https://msdn.microsoft.com/library/windows/apps/hh701582)

Mithilfe von Inhaltsübergangsanimationen können Sie den Inhalt eines Bildschirmbereichs ändern und gleichzeitig den Container oder Hintergrund unverändert lassen. Neuer Inhalt wird eingeblendet. Muss vorhandener Inhalt ersetzt werden, wird dieser Inhalt ausgeblendet.

## <span id="Recommendations"></span><span id="recommendations"></span><span id="RECOMMENDATIONS"></span>Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie eine Eingangsanimation, wenn mehrere neue Elemente in einen leeren Container aufgenommen werden sollen. Beispielsweise ist nach dem anfänglichen Laden einer App ein Teil des Inhalts nicht sofort für die Anzeige verfügbar. Wenn der Inhalt bereit zum Anzeigen ist, nehmen Sie diesen verzögerten Inhalt mithilfe einer Inhaltsübergangsanimation in die Ansicht auf.
-   Mit Inhaltsübergängen können Sie einen Satz Inhalt durch einen anderen ersetzen, der sich bereits im gleichen Container in einer Ansicht befindet.
-   Wenn Sie neuen Inhalt aufnehmen, ziehen Sie diesen (von unten nach oben) entgegen dem normalen Seitenfluss oder der Leserichtung in die Ansicht.
-   Stellen Sie neue Inhalte auf logische Art und Weise vor. Geben Sie beispielsweise die wichtigsten Inhalte zuletzt bekannt.
-   Wenn Sie den Inhalt mehrerer Container aktualisieren möchten, lösen Sie alle Übergangsanimationen ohne Staffelung oder Verzögerung gleichzeitig aus.
-   Verwenden Sie Inhaltsübergangsanimationen nicht, wenn sich die gesamte Seite ändert. Verwenden Sie in diesem Fall die Seitenübergangsanimationen.
-   Verwenden Sie Inhaltsübergangsanimationen nicht, wenn der Inhalt nur aktualisiert wird. Inhaltsübergangsanimationen sollen auf Bewegungen hinweisen. Verwenden Sie für Aktualisierungen Ein- und Ausblendungsanimationen.

\[Dieser Artikel enthält spezielle Informationen zu Apps für die universelle Windows-Plattform (UWP) und Windows 10. Laden Sie für Windows 8.1 die [PDF-Datei mit Windows 8.1-Richtlinien](https://go.microsoft.com/fwlink/p/?linkid=258743) herunter.\]

## <span id="related_topics"></span>Verwandte Artikel

**Für Entwickler (XAML)**
* [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animieren von Inhaltsübergängen](https://msdn.microsoft.com/library/windows/apps/xaml/jj649426)
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliothekanimationen](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**ContentThemeTransition-Klasse**](https://msdn.microsoft.com/library/windows/apps/br243104)

 

 






<!--HONumber=Mar16_HO3-->


