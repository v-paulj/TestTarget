---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Visuelle Ebene
description: Die Windows.UI.Composition-API ermöglicht den Zugriff auf die Kompositionsebene zwischen der Frameworkebene (XAML) und der Grafikebene (DirectX).
---
# Visuelle Ebene

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In Windows 10 wurde viel Arbeit in die Entwicklung eines neuen einheitlichen Kompositors und eines Renderingmoduls für alle Windows-Anwendungen (Desktopanwendungen und mobile Anwendungen) investiert. Daraus resultierte die einheitliche WinRT-Kompositions-API „Windows.UI.Composition“, die Zugriff auf neue einfache Kompositionsobjekte und neue kompositorgesteuerte Animationen und Effekte bietet.

„Windows.UI.Composition“ ist eine deklarative [Retained Mode](https://msdn.microsoft.com/library/windows/desktop/ff684178.aspx)-API, die aus jeder App der universellen Windows-Plattform (UWP) aufgerufen werden kann, um Kompositionsobjekte, Animationen und Effekte direkt in der Anwendung zu erstellen. Bei der API handelt es sich um eine leistungsstarke Ergänzung zu vorhandenen Frameworks wie beispielsweise XAML, die Entwicklern von UWP-Anwendungen eine vertraute C#-Oberfläche zur Erweiterung ihrer Anwendung bietet. Diese APIs können dazu verwendet werden, Anwendungen im DX-Stil ohne Frameworks zu erstellen.

Ein XAML-Entwickler kann sich auf die Kompositionsebene in C# begeben, um mit WinRT benutzerdefinierte Änderungen auf der Kompositionsebene vorzunehmen. So wird eine sogenannte „Kompositionsinsel“ von Objekten in ihrer XAML-Anwendung erstellt, und es wird vermieden, auf der Grafikebene mithilfe von DirectX und C++ benutzerdefinierte Änderungen an der Benutzeroberfläche vornehmen zu müssen.

![](images/layers-win-ui-composition.png)
## <span id="Composition_Objects_and_The_Compositor"></span><span id="composition_objects_and_the_compositor"></span><span id="COMPOSITION_OBJECTS_AND_THE_COMPOSITOR"></span>Kompositionsobjekte und der Kompositor

Kompositionsobjekte werden mit dem [**Kompositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) erstellt, der als Factory für Kompositionsobjekte fungiert. Der Kompositor kann [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekte erstellen, die wiederum die Erstellung einer visuellen Struktur ermöglichen, die die Grundlage für alle anderen Features und Kompositionsobjekte in der API bildet und von diesen verwendet wird.

Die API ermöglicht es Entwicklern, einzelne oder viele [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekte zu definieren und zu erstellen, die jeweils für einen einzelnen Knoten in einer visuellen Struktur stehen.

Visuelle Elemente können Container für andere visuelle Elemente sein oder Inhalte visueller Elemente hosten. Die API sorgt für mehr Benutzerfreundlichkeit, indem sie eine eindeutige Gruppe von [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)-Objekten für bestimmte Aufgaben bereitstellt, die in einer Hierarchie vorhanden sind:

-   [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) – Das Basisobjekt. Die meisten Eigenschaften befinden sich hier und werden von den anderen visuellen Objekten geerbt.
-   [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) – Abgeleitet von [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858). Bietet zusätzlich die Möglichkeit, untergeordnete visuelle Elemente einzufügen.
-   [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) – Abgeleitet von [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810). Enthält Bilder, Effekte und Swapchains.
-   [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) – Die Objektfactory, die die Beziehung zwischen einer Anwendung und dem Kompositorprozess des Systems verwaltet.

Der Kompositor ist auch eine Factory für zahlreiche andere Kompositionsobjekte, die dazu verwendet werden, um visuelle Elemente in der Struktur sowie eine Reihe von Animationen und Effekten zu beschneiden und umzuwandeln.

## <span id="Effects_System"></span><span id="effects_system"></span><span id="EFFECTS_SYSTEM"></span>Effektsystem

„Windows.UI.Composition“ unterstützt Echtzeiteffekte, die animiert, angepasst und verkettet werden können. Zu den Effekten zählen 2D-affine Transformationen, arithmetische Kompositionen, Mischungen, Farbquelle, Zusammensetzung, Kontrast, Belichtung, Graustufen, Gammakorrektur, Farbtondrehung, Invertierung, Sättigung, Sepia, Temperatur und Farbton.

Weitere Informationen finden Sie in der Übersicht [Kompositionseffekte](composition-effects.md).

## <span id="Animation_System"></span><span id="animation_system"></span><span id="ANIMATION_SYSTEM"></span>Animationssystem

„Windows.UI.Composition“ enthält ein ausdrucksstarkes, Framework-agnostisches Animationssystem, das Ihnen die Einrichtung von zwei Arten von Animationen ermöglicht: Keyframeanimationen und Ausdrucksanimationen. Diese werden dazu verwendet, visuelle Objekte zu verschieben, umzuwandeln oder zuzuschneiden bzw. einen Effekt zu animieren. Durch die direkte Ausführung im Kompositorprozess wird Gleichmäßigkeit und Skalierung erreicht, sodass Sie eine ganze Reihe eindeutiger Animationen gleichzeitig ausführen können.

Weitere Informationen finden Sie in der Übersicht [Kompositionsanimationen](composition-animation.md).

## <span id="XAML_Interoperation"></span><span id="xaml_interoperation"></span><span id="XAML_INTEROPERATION"></span>XAML-Interoperabilität

Die Composition-API kann eine visuelle Struktur von Grund auf neu erstellen und ist zusätzlich für die Interoperabilität mit einer bereits vorhandenen XAML-UI konzipiert, wobei die [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976)-Klasse in [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908) verwendet wird.


**Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <span id="Additional_Resources_"></span><span id="additional_resources_"></span><span id="ADDITIONAL_RESOURCES_"></span>Weitere Ressourcen:

-   Lesen Sie Kenny Kerrs MSDN-Artikel zu dieser API: [Grafiken und Animationen – Das Windows-Kompositionsmodul wird 10](https://msdn.microsoft.com/magazine/mt590968)
-   Kompositionsbeispiele finden Sie auf der [GitHub-Seite zum Thema „Komposition“](https://github.com/Microsoft/composition).
-   [**Ausführliche Referenzdokumentation für die API**](https://msdn.microsoft.com/library/windows/apps/Dn706878).
-   Bekannte Probleme: [Bekannte Probleme](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues).

 

 






<!--HONumber=Mar16_HO1-->


