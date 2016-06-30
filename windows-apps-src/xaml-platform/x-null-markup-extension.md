---
author: jwmsft
description: "Gibt im XAML-Markup einen NULL-Wert für eine Eigenschaft an."
title: xNull-Markuperweiterung
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 96ec27fa36d5a30d6bcf3b3c4ad4a330bf799a09

---

# {x:Null}-Markuperweiterung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Gibt im XAML-Markup einen **NULL**-Wert für eine Eigenschaft an.

## XAML-Attributsyntax

``` syntax
<object property="{x:Null}" .../>
```

## Hinweise

**null** ist das Nullverweis-Schlüsselwort für C# und C++. Das Microsoft Visual Basic-Schlüsselwort für einen Nullverweis lautet **Nothing**.

Der anfängliche Standardwert kann zwischen Abhängigkeitseigenschaften variieren und ist nicht unbedingt **null**. Viele Abhängigkeitseigenschaften akzeptieren **null** außerdem aufgrund ihrer internen Implementierung nicht als Wert (weder per Markup noch per Code). In einem solchen Fall tritt unter Umständen eine Analyseausnahme auf, wenn ein XAML-Attributwert mit **{x:Null}** festgelegt wird.

Einige Windows-Runtime-Typen akzeptieren NULL-Werte. Sollte bei einem Typ, der NULL-Werte akzeptiert, **null** nicht bereits als Standardwert festgelegt sein, können Sie **{x:Null}** in XAML verwenden, um den **NULL**-Wert festzulegen. Bei Verwendung von Visual C++-Komponentenerweiterungen (C++/CX) werden Typen, die NULL-Werte akzeptieren, als [**Platform::IBox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx) dargestellt. In Microsoft .NET-Sprachen werden Typen, die NULL-Werte akzeptieren, mit [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx) angegeben.

## Verwandte Themen

* [**Null-Wert<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 




<!--HONumber=Jun16_HO4-->


