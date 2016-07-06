---
author: jwmsft
description: Stellt eine Methode bereit, um die Quelle einer Bindung als relative Beziehung im Laufzeitobjektdiagramm anzugeben.
title: RelativeSource-Markuperweiterung
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.sourcegitcommit: ec4c9b87655425e82a1cb792d0acc6bee265e9d2
ms.openlocfilehash: 9f0bb49e701806f8635d93fa495cdab6486a4ea3

---

# {RelativeSource}-Markuperweiterung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Stellt eine Methode bereit, um die Quelle einer Bindung als relative Beziehung im Laufzeitobjektdiagramm anzugeben.

## Verwendung von XAML-Attributen (Self-Modus)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## Verwendung von XAML-Attributen (TemplatedParent-Modus)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## XAML-Werte

| Begriff | Beschreibung |
|------|-------------|
| {RelativeSource Self} | Erzeugt den [<strong>Mode</strong>](https://msdn.microsoft.com/library/windows/apps/br209915)-Wert <strong>Self</strong>. Das Zielelement sollte als Quelle für diese Bindung verwendet werden. Dies ist nützlich, wenn eine Eigenschaft eines Elements an eine andere Eigenschaft im gleichen Element gebunden werden soll. |
| {RelativeSource TemplatedParent} | Erzeugt eine [<strong>ControlTemplate</strong>](https://msdn.microsoft.com/library/windows/apps/br209391), die als Quelle für diese Bindung angewendet wird. Dies ist nützlich, wenn Laufzeitinformationen in Bindungen auf Vorlagenebene angewendet werden sollen. | 

## Anmerkungen

Mit einer [**Bindung**](https://msdn.microsoft.com/library/windows/apps/br209820) kann [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) entweder als Attribut für ein **Binding**-Objektelement oder als Komponente in einer [{Binding}-Markuperweiterung](binding-markup-extension.md) festgelegt werden. Aus diesem Grund werden zwei verschiedene Varianten der XAML-Syntax dargestellt.

**RelativeSource** ähnelt der [{Binding}-Markuperweiterung](binding-markup-extension.md).  Hierbei handelt es sich um eine Markuperweiterung, die Instanzen selbstständig zurückgeben kann und die eine zeichenfolgenbasierte Konstruktion unterstützt, die im Wesentlichen ein Argument an den Konstruktor übergibt. In diesem Fall ist das Argument, das übergeben wird, der [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209915)-Wert.

Der **Self**-Modus ist nützlich, wenn eine Eigenschaft eines Elements an eine andere Eigenschaft im gleichen Element gebunden werden soll. Dies stellt außerdem eine Variation der [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828)-Bindung dar, erfordert aber keine Benennung und keinen anschließenden Selbstverweis des Elements. Wenn Sie eine Eigenschaft eines Elements an eine andere Eigenschaft im gleichen Element binden, müssen die Eigenschaften den gleichen Eigenschaftentyp ausweisen. Andernfalls müssen Sie auch einen [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) für die Bindung verwenden, um die Werte zu konvertieren. Sie können beispielsweise [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) als Quelle für [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) ohne Konvertierung verwenden. Sie benötigen jedoch einen Konverter, um [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) als Quelle für [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006) zu nutzen.

Im Folgenden finden Sie ein Beispiel hierfür. Dieses [**Rechteck**](https://msdn.microsoft.com/library/windows/apps/br243371) verwendet eine [{Binding}-Markuperweiterung](binding-markup-extension.md), damit [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) und [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) immer gleich sind und eine Darstellung als Quadrat erfolgt. Nur die Höhe wird als fester Wert festgelegt. Der standardmäßige [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) dieses **Rechtecks** ist **null** anstelle von **this**. Daher verwenden wir das `RelativeSource={RelativeSource Self}`-Argument in der Syntax der {Binding}-Markuperweiterung, um die Datenkontextquelle auf das eigentliche Objekt festzulegen (und die Bindung an ihre anderen Eigenschaften zu ermöglichen).

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Darüber hinaus kann `RelativeSource={RelativeSource Self}` auch verwendet werden, um den [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) eines Objekts auf sich selbst festzulegen.  Dieses Verfahren kommt etwa in einigen der SDK-Beispiele zum Einsatz, bei denen die [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)-Klasse mit einer benutzerdefinierten Eigenschaft erweitert wurde, die bereits ein verwendungsbereites Modell für die eigene Datenbindung bereitstellt. Beispiel: `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Hinweis:** Die Verwendung von **RelativeSource** in XAML zeigt nur die beabsichtigte Verwendung, nämlich das Festlegen eines Werts für [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) in XAML als Teil eines Bindungsausdrucks. Theoretisch sind andere Verwendungen möglich, wenn Sie eine Eigenschaft mit einem [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)-Wert festlegen.

## Verwandte Themen

* [Übersicht über XAML](xaml-overview.md)
* [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [{Binding}-Markuperweiterung](binding-markup-extension.md)
* [**Bindung**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)




<!--HONumber=Jun16_HO5-->


