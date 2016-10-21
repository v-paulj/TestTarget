---
author: TylerMSFT
title: "Hinzufügen eines Begrüßungsbildschirms"
description: "Hier erfahren Sie, wie Sie das Bild und die Hintergrundfarbe für den Begrüßungsbildschirm der App mit Microsoft Visual Studio 2015 festlegen."
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
translationtype: Human Translation
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 4d8a00cf7bd151ab97e9abc10a09a3794a0e292f

---

# Hinzufügen eines Begrüßungsbildschirms


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Hier erfahren Sie, wie Sie das Bild und die Hintergrundfarbe für den Begrüßungsbildschirm der App mit Microsoft Visual Studio 2015 festlegen.

## Festlegen von Bild und Hintergrundfarbe für den Begrüßungsbildschirm mit Visual Studio 2015


Wenn Sie eine Visual Studio 2015-Vorlage zum Erstellen der App verwenden, wird dem Projekt ein Standardbild hinzugefügt und als Bild für den Begrüßungsbildschirm festgelegt. Als Hintergrundfarbe für den Begrüßungsbildschirm wird standardmäßig Hellgrau verwendet. Führen Sie die folgenden Schritte aus, wenn Sie das Standardbild oder die Farbe des App-Begrüßungsbildschirms ändern möchten:

1.  Öffnen Sie das vorhandene UWP-App-Projekt (Universelle Windows-Plattform) in Visual Studio 2015.
2.  Öffnen Sie im **Projektmappen-Explorer** die Datei „Package.appxmanifest“. Sie können diese Datei auch über die Menüleiste öffnen, indem Sie **Projekt** &gt; **Store** &gt; **App-Manifest bearbeiten** auswählen.
3.  Öffnen Sie die Registerkarte **Visuelle Anlagen**, und wählen Sie links im Fenster „Package.appxmanifest“ unter **Alle Bildanlagen** die Option **Begrüßungsbildschirm**. Falls Sie den Begrüßungsbildschirm zum ersten Mal ändern, wird im Feld **Begrüßungsbildschirm** der Pfad „Assets\\SplashScreen.png“ angezeigt.

    Im folgenden Screenshot ist das Fenster „Package.appxmanifes“ in Visual Studio 2015 dargestellt. Je nach Art des Projekts werden leicht unterschiedliche Sätze von visuellen Ressourcen angezeigt.

    ![Screenshot des Fensters „Package.appxmanifest“ in Visual Studio 2013](images/appmanifest.png)

    Wenn Sie „Package.appxmanifest“ in einem Text-Editor öffnen, wird das [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467)-Element als untergeordnetes Element des [**VisualElements**](https://msdn.microsoft.com/library/windows/apps/br211471)-Elements angezeigt. Das standardmäßige Begrüßungsbildschirm-Markup in der Manifestdatei sieht in einem Text-Editor wie folgt aus:

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4.  Drücken Sie zum Auswählen eines neuen Begrüßungsbildschirm-Bilds für eine UWP-App die Schaltfläche mit den Auslassungspunkten, die neben der Bezeichnung **1240 x 600 px** unterhalb von **Skalierte Anlagen** angezeigt wird. Wählen Sie das Bild mit 1240 x 600 Pixeln (PNG, JPG oder JPEG) aus, das Sie für den Begrüßungsbildschirm verwenden möchten.

    **Wichtig**  Das Bild für den Begrüßungsbildschirm muss 620x300 Pixel groß sein und einen einfachen Skalierungsfaktor aufweisen. Beachten Sie beim Entwerfen des Begrüßungsbildschirms zudem, dass er kleiner als der Bildschirm und zentriert angezeigt wird. Im Gegensatz zum Begrüßungsbildschirm einer Windows Phone Store-App füllt er den Bildschirm nicht komplett aus.

     

5.  Drücken Sie zum Auswählen eines neuen Bilds für eine Windows Phone Store-App die Schaltfläche mit den Auslassungszeichen, die neben der Bezeichnung **1152 x 1920 px** unterhalb von **Skalierte Anlagen** angezeigt wird. Wählen Sie das Bild mit 1152 x 1920 Pixeln (.png, .jpg oder .jpeg) aus, das Sie für den Begrüßungsbildschirm verwenden möchten.

    **Wichtig**  Das Bild für den Begrüßungsbildschirm muss 1152x1920 Pixel aufweisen. Dabei handelt es sich um die richtige Größe für einen Skalierungsfaktor von 2,4. Ist dies die einzige Ressource, die Sie bereitstellen, wird es auf die Skalierungsfaktoren 1.4x und 1x nach unten skaliert.

     

6.  Legen Sie im Abschnitt **Begrüßungsbildschirm** im Feld **Hintergrundfarbe** die Hintergrundfarbe fest, die zusammen mit Ihrem Bild angezeigt werden soll. Sie können entweder einen Farbnamen oder „\#“ und den Hexadezimalwert einer Farbe eingeben. Eine Liste mit den Namen der verfügbaren Farben finden Sie unter [**SplashScreen-Element**](https://msdn.microsoft.com/library/windows/apps/br211467).

    Das Festlegen einer Hintergrundfarbe für Ihren Begrüßungsbildschirm ist nicht unbedingt erforderlich. Wenn Sie keine Farbe für eine UWP-App (Universelle Windows-Plattform) angeben, wird als Hintergrundfarbe für den Begrüßungsbildschirm standardmäßig Hellgrau (Hexadezimalwert \#464646) verwendet. Dies ist die gleiche Farbe wie die standardmäßige Hintergrundfarbe für eine **Kachel** (siehe Feld **Hintergrundfarbe** im Abschnitt **Bilder für Kacheln und Logos** auf der Registerkarte **Visuelle Anlagen**). Wenn Sie keine Farbe für ein Windows Phone angeben oder „transparent“ festlegen, wird die Hintergrundfarbe des Begrüßungsbildschirms transparent sein.

## Zusammenfassung und nächste Schritte


Falls das Laden der App einige Zeit dauert, können Sie erwägen, einen erweiterten Begrüßungsbildschirm hinzuzufügen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Erstellen eines benutzerdefinierten Begrüßungsbildschirms](create-a-customized-splash-screen.md).

**Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Verwandte Themen

* [Erstellen eines benutzerdefinierten Begrüßungsbildschirms](create-a-customized-splash-screen.md)

**Referenzen**

* [**Schemareferenz zum Paketmanifest: SplashScreen-Element**](https://msdn.microsoft.com/library/windows/apps/br211467)
* [**Windows.ApplicationModel.Activation.SplashScreen-Klasse**](https://msdn.microsoft.com/library/windows/apps/br224763)

 

 



<!--HONumber=Aug16_HO3-->


