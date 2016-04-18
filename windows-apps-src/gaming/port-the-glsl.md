---
title: Portieren des GLSL-Codes
description: Nachdem Sie sich um den Code gekümmert haben, mit dem die Puffer und Shaderobjekte erstellt und konfiguriert werden, muss der in diesen Shadern enthaltene Code von der GL Shader Language (GLSL) von OpenGL ES 2.0 in die High-Level Shader Language (HLSL) von Direct3D 11 portiert werden.
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
---

# Portieren des GLSL-Codes


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Wichtige APIs**

-   [HLSL-Semantik](https://msdn.microsoft.com/library/windows/desktop/bb205574)
-   [Shaderkonstanten (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)

Nachdem Sie sich um den Code gekümmert haben, mit dem die Puffer und Shaderobjekte erstellt und konfiguriert werden, muss der in diesen Shadern enthaltene Code von der GL Shader Language (GLSL) von OpenGL ES 2.0 in die High-Level Shader Language (HLSL) von Direct3D 11 portiert werden.

Unter OpenGL ES 2.0 geben Shader Daten nach der Ausführung zurück, indem systeminterne Elemente wie **gl\_Position**, **gl\_FragColor** oder **gl\_FragData\[n\]** verwendet werden (wobei „n“ der Index für ein bestimmtes Renderziel ist). In Direct3D sind keine speziellen systeminternen Funktionen vorhanden. Von den Shadern werden Daten als Rückgabetyp ihrer jeweiligen main()-Funktionen zurückgegeben.

Daten, die zwischen Shaderphasen interpoliert werden sollen, z. B. Vertexposition oder Normale, werden mithilfe der **varying**-Deklaration behandelt. Direct3D verfügt jedoch nicht über diese Deklaration. Alle Daten, die zwischen Shaderphasen übertragen werden sollen, müssen daher per [HLSL-Semantik](https://msdn.microsoft.com/library/windows/desktop/bb205574) markiert werden. Mit der jeweils gewählten Semantik wird der Zweck für die Daten angegeben. Vertexdaten, die zwischen Fragmentshadern interpoliert werden sollen, werden beispielsweise wie folgt deklariert:

`float4 vertPos : POSITION;`

oder

`float4 vertColor : COLOR;`

POSITION steht hierbei für die Semantik, die zum Angeben der Vertexpositionsdaten verwendet wird. POSITION stellt außerdem einen Sonderfall dar, da vom Pixelshader nach der Interpolation darauf nicht zugegriffen werden kann. Daher müssen Sie Eingaben für den Pixelshader mithilfe von SV\_POSITION angeben. Die interpolierten Vertexdaten werden dann in diese Variable eingefügt.

`float4 position : SV_POSITION;`

Die Semantik kann für die body-Methoden (main) von Shadern deklariert werden. Für Pixelshader ist für die body-Methode SV\_TARGET\[n\] erforderlich. Damit wird ein Renderziel angegeben. (SV\_TARGET ohne numerisches Suffix ergibt standardmäßig den Renderzielindex 0.)

Beachten Sie auch, dass Vertex-Shader erforderlich sind, um die SV\_POSITION-Systemwertsemantik auszugeben. Mit dieser Semantik werden die Vertexpositionsdaten zu Koordinatenwerten aufgelöst, wobei "x" zwischen -1 und 1 und "y" zwischen -1 und 1 liegt. "z" wird durch den ursprünglichen homogenen Koordinatenwert "w" dividiert (z/w), und "w" ist 1 dividiert durch den Originalwert "w" (1/w). Für Pixelshader wird die SV\_POSITION-Systemwertsemantik zum Abrufen der Pixelposition auf dem Bildschirm verwendet, wobei "x" zwischen 0 und der Breite des Renderziels und "y" zwischen 0 und der Höhe des Renderziels liegt (jeweils um den Wert 0,5 versetzt). Pixelshader der Featureebene 9\_x können nicht aus dem SV\_POSITION-Wert auslesen.

Konstantenpuffer müssen mit **cbuffer** deklariert und mit einem speziellen Startregister für die Suche versehen werden.

Direct3D 11: Deklaration eines HLSL-Konstantenpuffers

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

Hier wird vom Konstantenpuffer das Register „b0“ für den gepackten Puffer verwendet. In der Form "b\#" wird auf alle Register verwiesen. Weitere Informationen zur HLSL-Implementierung von Konstantenpuffern, Registern und Datenpaketen finden Sie unter [Shaderkonstanten (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581).

Anweisungen
------------

### Schritt 1: Portieren des Vertex-Shaders

In diesem einfachen OpenGL ES 2.0-Beispiel verfügt der Vertex-Shader über drei Eingaben: eine konstante Modell-Ansicht-Projektion-4x4-Matrix und zwei Vektoren mit vier Koordinaten. Diese beiden Vektoren enthalten die Vertexposition und ihre Farbe. Der Shader wandelt den Positionsvektor in Perspektivenkoordinaten um und weist ihn der systeminternen gl\_Position-Funktion zur Rasterung zu. Außerdem wird die Vertexfarbe zur Interpolation während der Rasterung in eine abweichende Variable kopiert.

OpenGL ES 2.0: Vertex-Shader für das Würfelobjekt (GLSL)

``` syntax
uniform mat4 u_mvpMatrix; 
attribute vec4 a_position;
attribute vec4 a_color;
varying vec4 destColor;

void main()
{           
  gl_Position = u_mvpMatrix * a_position;
  destColor = a_color;
}
```

In Direct3D ist die konstante Modell-Ansicht-Projektion-Matrix in einem Konstantenpuffer enthalten, der unter Register b0 verpackt ist, und die Vertexposition und -farbe sind speziell mit der jeweils geeigneten HLSL-Semantik gekennzeichnet: POSITION und COLOR. Da das Eingabelayout eine bestimmte Anordnung dieser beiden Vertexwerte vorgibt, erstellen Sie dafür eine Struktur und deklarieren diese als Typ für den Eingabeparameter der body-Shaderfunktion (main). (Sie können die Werte auch als zwei einzelne Parameter angeben, was mitunter jedoch umständlich ist.) Außerdem geben Sie einen Ausgabetyp für diese Phase an, der die interpolierte Position und Farbe enthält, und deklarieren ihn als Rückgabewert für die body-Funktion des Vertex-Shaders.

Direct3D 11: Vertex-Shader für das Würfelobjekt (HLSL)

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float3 pos : SV_POSITION;
  float3 color : COLOR;
};

PixelShaderInput main(VertexShaderInput input)
{
  PixelShaderInput output;
  float4 pos = float4(input.pos, 1.0f); // add the w-coordinate

  pos = mul(mvp, projection);
  output.pos = pos;

  output.color = input.color;

  return output;
}
```

Der Ausgabedatentyp „PixelShaderInput“ wird während der Rasterung aufgefüllt und für den Fragmentshader (Pixelshader) bereitgestellt.

### Schritt 2: Portieren des Fragmentshaders

Der Fragmentshader in GLSL im Beispiel ist sehr einfach gestaltet: Der interpolierte Farbwert wird für die systeminterne gl\_FragColor-Funktion bereitgestellt. Von OpenGL ES 2.0 wird er in das standardmäßige Renderziel geschrieben.

OpenGL ES 2.0: Fragmentshader für das Würfelobjekt (GLSL)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Für Direct3D ist der Aufbau nahezu genauso einfach. Der einzige zu beachtende Unterschied besteht darin, dass die body-Funktion des Pixelshaders einen Wert zurückgeben muss. Da es sich bei der Farbe um einen Gleitkommawert mit vier Koordinaten (RGBA) handelt, geben Sie float4 als Rückgabetyp an. Geben Sie anschließend das standardmäßige Renderziel als SV\_TARGET-Systemwertsemantik an.

Direct3D 11: Pixelshader für das Würfelobjekt (HLSL)

``` syntax
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR;
};


float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color, 1.0f);
}
```

Die Farbe für das Pixel an der Position wird in das Renderziel geschrieben. Das Anzeigen der Inhalte dieses Renderziels als nächster Schritt wird unter [Zeichnen auf den Bildschirm](draw-to-the-screen.md) beschrieben.

## Vorheriger Schritt


[Portieren der Vertexpuffer und -daten](port-the-vertex-buffers-and-data-config.md)
Nächster Schritt
---------

[Zeichnen auf den Bildschirm](draw-to-the-screen.md)
Anmerkungen
-------

Wenn Sie mit der HLSL-Semantik und dem Packen von Konstantenpuffern vertraut sind, können Sie einigen Debugaufwand vermeiden und Möglichkeiten zur Optimierung schaffen. Lesen Sie sich nach Möglichkeit die Themen [Variablensyntax (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509706), [Einführung in Puffer in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898) und [Erstellen eines Konstantenpuffers](https://msdn.microsoft.com/library/windows/desktop/ff476896) durch. Hier sind als Anfang schon einmal einige Tipps aufgeführt, die in Verbindung mit der Semantik und Konstantenpuffern zu beachten sind:

-   Überprüfen Sie stets den Direct3D-Konfigurationscode des Renderers, um sicherzustellen, dass die Strukturen für die Konstantenpuffer mit den cbuffer-Strukturdeklarationen der HLSL übereinstimmen und dass die Komponentenskalartypen für beide Deklarationen übereinstimmen.
-   Verwenden Sie im C++-Code des Renderers [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)-Typen in den Konstantenpufferdeklarationen, um für die richtige Verpackung der Daten zu sorgen.
-   Die beste Möglichkeit zur effizienten Nutzung von Konstantenpuffern ist die Organisation von Shadervariablen in Konstantenpuffern anhand der Updatehäufigkeit. Wenn Sie beispielsweise über uniform-Daten verfügen, die einmal pro Frame aktualisiert werden, sowie über andere uniform-Daten, die nur bei Kamerabewegungen aktualisiert werden, empfiehlt sich das Aufteilen der Daten auf zwei separate Konstantenpuffer.
-   Semantik, deren Anwendung Sie vergessen oder die Sie auf fehlerhafte Weise angewendet haben, ist die wichtigste Ursache für Fehler der Shaderkompilierung (FXC). Deshalb sollten Sie die Semantik auf jeden Fall noch einmal überprüfen. Die Dokumente können etwas verwirrend erscheinen, da viele ältere Seiten und Beispiele noch auf andere Versionen der HLSL-Semantik des Stands vor Direct3D 11 verweisen.
-   Stellen Sie sicher, dass Sie wissen, auf welche Direct3D-Featureebene Sie für einen Shader jeweils abzielen. Die Semantik für Featureebene "9\_\*" unterscheidet sich von der Semantik für 11\_1.
-   Mit der SV\_POSITION-Semantik werden die zugeordneten Daten für die Position nach der Interpolation zu Koordinatenwerten aufgelöst, wobei „x“ zwischen 0 und der Breite des Renderziels und „y“ zwischen 0 und der Höhe des Renderziels liegt. „z“ wird durch den ursprünglichen homogenen Koordinatenwert „w“ dividiert (z/w). „w“ ist 1 dividiert durch den Originalwert „w“ (1/w).

## Verwandte Themen


[Portieren eines einfachen OpenGL ES 2.0-Renderers zu Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Portieren der Shaderobjekte](port-the-shader-config.md)

[Portieren der Vertexpuffer und Daten](port-the-vertex-buffers-and-data-config.md)

[Zeichnen auf den Bildschirm](draw-to-the-screen.md)

 

 






<!--HONumber=Mar16_HO1-->


