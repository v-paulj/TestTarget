---
author: jwmsft
description: "Ändert das XAML-Kompilierungsverhalten, sodass Felder für Verweise auf benannte Objekte mit öffentlichem Zugriff definiert werden und nicht das private Standardverhalten aufweisen."
title: xFieldModifier-Attribut
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3f6c759d582d92aa33df5874186aeaa3a7c37b01

---

# x:FieldModifier-Attribut

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Ändert das XAML-Kompilierungsverhalten, sodass Felder für Verweise auf benannte Objekte mit **öffentlichem** Zugriff definiert werden und nicht das **private** Standardverhalten aufweisen.

## XAML-Attributsyntax

``` syntax
<object x:FieldModifier="public".../>
```

## Abhängigkeiten


            [x:Name-Attribut](x-name-attribute.md) muss in demselben Element ebenfalls bereitgestellt werden.

## Hinweise

Der Wert für das **x:FieldModifier**-Attribut variiert je nach Programmiersprache. Welche Zeichenfolge verwendet wird, hängt davon ab, wie eine Sprache ihren **CodeDomProvider** implementiert und welche Typkonverter zurückgegeben werden, um die Bedeutungen für **TypeAttributes.Public** und **TypeAttributes.NotPublic** zu definieren. Für C#-, Microsoft Visual Basic- oder VisualC++-Komponentenerweiterungen (C++/CX) können Sie den Zeichenfolgenwert „public“ oder „Public“ angeben. Vom Parser wird die Groß-/Kleinschreibung für diesen Attributwert nicht berücksichtigt.

Sie können auch **NonPublic** (**internal** in C# oder C++/CX, **Friend** in Visual Basic) angeben, aber dies ist unüblich. Der "interne" Zugriff gilt nicht für das Modell zur Generierung von Windows-Runtime-XAML-Code. Standardmäßig wird der „private“ Zugriff verwendet.


            **x:FieldModifier** ist nur für Elemente mit einem [x:Name-Attribut](x-name-attribute.md) relevant, weil unter diesem Namen auf das Feld verwiesen wird, nachdem dieses den Zustand „public“ erreicht hat.


            **Hinweis**  Windows-Runtime-XAML unterstützt weder **x:ClassModifier** noch **x:Subclass**.




<!--HONumber=Jun16_HO4-->


