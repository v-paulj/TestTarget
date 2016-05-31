---
author: mtoepke
title: Visual Studio-Tools für die Spieleprogrammierung
description: Sie erhalten eine Übersicht über spezielle DirectX-Tools, die unter Visual Studio verfügbar sind.
ms.assetid: 43137bfc-7876-70e0-515c-4722f68bd064
---

# Visual Studio-Tools für die Spieleprogrammierung


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Zusammenfassung**

-   [Erstellen eines DirectX-Spieleprojekts aus einer Vorlage](user-interface.md)
-   Visual Studio-Tools für die Programmierung von DirectX-Spielen


Wenn Sie Visual Studio Ultimate zum Entwickeln von DirectX-Apps verwenden, können Sie zusätzliche Tools zum Erstellen, Bearbeiten, Anzeigen in der Vorschau und Exportieren von Bild-, Modell- und Shaderressourcen nutzen. Außerdem sind Tools verfügbar, mit denen Sie Ressourcen zur Erstellungszeit konvertieren und DirectX-Grafikcode debuggen können.

Dieses Thema enthält eine Übersicht über diese Grafiktools.

## Grafik-Editor


Nutzen Sie den Grafik-Editor zum Arbeiten mit den Arten von umfassenden Textur- und Bildformaten, die von DirectX verwendet werden. Der Grafik-Editor unterstützt die folgenden Formate:

-   PNG
-   JPG, JPEG, JPE, JFIF
-   DDS
-   GIF
-   BMP
-   DIB
-   TIF, TIFF
-   TGA

Erstellen Sie [Buildanpassungsdateien](#custom), um diese Formate zur Buildzeit in DDS-Dateien zu konvertieren.

Weitere Informationen finden Sie unter [Arbeiten mit Texturen und Bildern](https://msdn.microsoft.com/library/windows/apps/hh873119.aspx).

> **Hinweis**  Der Grafik-Editor ist nicht als Ersatz für eine Bildbearbeitungs-App mit vollem Funktionsumfang gedacht, aber für viele einfache Anzeige- und Bearbeitungsfälle geeignet.

 

## Modell-Editor


Sie können den Modell-Editor verwenden, um grundlegende 3D-Modelle ganz neu zu erstellen oder um komplexere 3D-Modelle aus umfassenden 3D-Modelliertools anzuzeigen oder zu ändern. Der Modell-Editor unterstützt mehrere 3D-Modellformate, die bei der Entwicklung von DirectX-Apps verwendet werden. Sie können [Buildanpassungsdateien](#custom) erstellen, um diese Formate zur Buildzeit in CMO-Dateien zu konvertieren.

-   FBX
-   DAE
-   OBJ

Dies ist ein Screenshot eines Modells im Editor, auf das Beleuchtungsfunktionen angewendet wurden.

![Teekanne](images/modeleditor.png)

Weitere Informationen finden Sie unter [Arbeiten mit 3D-Modellen](https://msdn.microsoft.com/library/windows/apps/hh873114.aspx).

> **Hinweis**  Der Modell-Editor ist nicht als Ersatz für eine Modellbearbeitungs-App mit vollem Funktionsumfang gedacht, aber für viele einfache Anzeige- und Bearbeitungsfälle geeignet.

 

## Shader-Designer


Verwenden Sie den Shader-Designer zum Erstellen benutzerdefinierter visueller Effekte für das Spiel oder die App, auch wenn Sie nicht mit der HLSL-Programmierung vertraut sind.

Sie erstellen einen Shader visuell als Diagramm. Jeder Knoten zeigt eine Vorschau der Ausgabe bis zum jeweiligen Vorgang an. Im nächsten Beispiel wird Lambert-Beleuchtung mit einer Kugelvorschau angewendet.

![Visuelles Shaderdiagramm](images/shaderdesigner.png)

Verwenden Sie den Shader-Editor, um Shader im DGSL-Format zu entwerfen, zu bearbeiten und zu speichern. Außerdem können die folgenden Formate exportiert werden:

-   HLSL (Quellcode)
-   CSO (Bytecode)
-   H (HLSL-Bytecode-Array)

Erstellen Sie [Buildanpassungsdateien](#custom), um diese Formate zur Buildzeit in CSO-Dateien zu konvertieren.

Unten ist ein Abschnitt eines HLSL-Codes angegeben, der vom Shader-Editor exportiert wurde. Dies ist nur der Code für den Lambert-Beleuchtungsknoten.

```hlsl
//
// Lambert lighting function
//
float3 LambertLighting(
    float3 lightNormal,
    float3 surfaceNormal,
    float3 materialAmbient,
    float3 lightAmbient,
    float3 lightColor,
    float3 pixelColor
    )
{
    // Compute the amount of contribution per light.
    float diffuseAmount = saturate(dot(lightNormal, surfaceNormal));
    float3 diffuse = diffuseAmount * lightColor * pixelColor;

    // Combine ambient with diffuse.
    return saturate((materialAmbient * lightAmbient) + diffuse);
}
```

Weitere Informationen finden Sie unter [Arbeiten mit Shadern](https://msdn.microsoft.com/library/windows/apps/hh873117.aspx).

## Buildanpassungen für 3D-Objekte


Sie können dem Projekt Buildanpassungen hinzufügen, sodass Ressourcen von Visual Studio in nutzbare Formate konvertiert werden. Danach können Sie die Objekte in die App laden und verwenden, indem Sie DirectX-Ressourcen wie in jeder anderen DirectX-App auch erstellen und füllen.

Zum Hinzufügen von Buildanpassungen klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt und wählen die Option **Buildanpassungen...**. Sie können dem Projekt die folgenden Arten von Buildanpassungen hinzufügen:

-   Von der Bildinhaltpipeline werden Bilddateien als Eingaben verwendet und DirectDraw Surface-Dateien (.dds) ausgegeben.
-   Von der Gitterinhaltpipeline werden Gitterdateien (z. B. FBX) als Eingabe verwendet und CMO-Gitterdateien ausgegeben.
-   Von der Shaderinhaltpipeline werden visuelle Shaderdiagramme (.dgsl) aus dem Shader-Editor von Visual Studio verwendet und kompilierte Shaderausgabedateien (.cso) ausgegeben.

Weitere Informationen finden Sie unter [Verwenden von 3D-Objekten im Spiel oder in der App](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx).

## Debuggen von DirectX-Grafiken


Visual Studio enthält grafikspezifische Debugtools. Verwenden Sie diese Tools zum Debuggen der folgenden Komponenten:

-   Grafikpipeline
-   Ereignisaufrufliste
-   Objekttabelle
-   Gerätestatus
-   Shaderfehler
-   Nicht initialisierte oder fehlerhafte Konstantenpuffer und Parameter
-   DirectX-Versionskompatibilität
-   Begrenzte Direct2D-Unterstützung
-   Betriebssystem- und SDK-Anforderungen

Weitere Informationen finden Sie unter [Debuggen von DirectX-Grafiken](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx).

> **Hinweis**  Dieser Artikel ist für Windows 10-Entwickler gedacht, die Apps für die Universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 






<!--HONumber=May16_HO2-->


