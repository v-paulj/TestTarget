---
author: mtoepke
title: Zuordnen von OpenGLES2.0 zu Direct3D11
description: Machen Sie sich zu Beginn des Prozesses zur ersten Portierung Ihrer Grafikarchitektur von OpenGL ES 2.0 zu Direct3D mit den Hauptunterschieden zwischen den APIs vertraut.
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d21bbf905797a7b0c14e666f1ec31a85203b30db

---

# Zuordnen von OpenGLES2.0 zu Direct3D11


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Machen Sie sich zu Beginn des Prozesses zur ersten Portierung Ihrer Grafikarchitektur von OpenGL ES 2.0 zu Direct3D mit den Hauptunterschieden zwischen den APIs vertraut. Mithilfe der Themen in diesem Abschnitt können Sie die Portierungsstrategie und die API-Änderungen planen, die erforderlich sind, wenn Sie die Grafikverarbeitung auf Direct3D umstellen.
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Planen der Portierung von OpenGLES2.0 zu Direct3D](compare-opengl-es-2-0-api-design-to-directx.md)</p></td>
<td align="left"><p>Wenn Sie ein Spiel von der iOS- oder Android-Plattform portieren, haben Sie vermutlich erheblich in OpenGLES2.0 investiert. Beim Vorbereiten der Portierung Ihrer Grafikpipeline-Codebasis zu Direct3D11 und der Windows-Runtime sind im Vorfeld einige Dinge zu beachten.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Vergleichen des EGL-Codes mit DXGI und Direct3D](moving-from-egl-to-dxgi.md)</p></td>
<td align="left"><p>Die DirectX-Grafikschnittstelle (DXGI) und verschiedene Direct3D-APIs erfüllen die gleiche Rolle wie EGL. In diesem Thema werden die DXGI und Direct3D11 aus Sicht von EGL erläutert.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Vergleichen von OpenGLES2.0-Puffern, -uniform-Elementen und -Vertexattributen mit Direct3D](porting-uniforms-and-attributes.md)</p></td>
<td align="left"><p>Während des Portierens zu Direct3D 11 aus OpenGL ES 2.0 müssen Sie die Syntax und das API-Verhalten zum Übergeben von Daten zwischen der App und den Shaderprogrammen ändern.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Vergleich der OpenGLES2.0-Shaderpipeline mit Direct3D](change-your-shader-loading-code.md)</p></td>
<td align="left"><p>Vom Konzept her ist die Direct3D11-Shaderpipeline der in OpenGLES2.0 sehr ähnlich. Hinsichtlich des API-Entwurfs sind die Hauptkomponenten für die Erstellung und Verwaltung der Shaderstufen jedoch Teile der zwei primären Schnittstellen [<strong>ID3D11Device1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404575) und [<strong>ID3D11DeviceContext1</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404598). In diesem Thema versuchen wir, allgemeine Muster der OpenGLES2.0-Shaderpipeline-API den Direct3D11-Entsprechungen in diesen Schnittstellen zuzuordnen.</p></td>
</tr>
</tbody>
</table>

 

## Hinweise zu bestimmten OpenGLES2.0-Anbietern


In diesen Themen wird die OpenGLES2.0-Spezifikation von Khronos mit der plattformagnostischen ProgrammierspracheC verwendet. Sowohl von iOS als auch von Android wird dieselbe Spezifikation genutzt, und der für diese Plattformen entwickelte OpenGLES2.0-Code weist große Ähnlichkeit mit den Codeausschnitten auf, die wir durchgehen werden, obwohl diese meist als objektorientierte APIs verfügbar gemacht werden. Aufgrund der Eigenheiten und Sprachunterschiede der einzelnen Plattformen können geringe Unterschiede bestehen, vor allem bei den Typen der Methodenparameter oder bei der allgemeinen Sprachsyntax. Für iOS wird beispielsweise Objective-C genutzt. Für Android kann C++ verwendet werden. Einige Entwickler verlassen sich jedoch möglicherweise auf eine reine Java-Implementierung. Trotz dieser Unterschiede sind diese Themen hilfreich, da bei den allgemeinen Konzepten, dem Aufbau und der Verwendung der OpenGLES-APIs keine Abweichungen bestehen.

 

 







<!--HONumber=Aug16_HO3-->


