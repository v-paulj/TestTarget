---
author: mijacobs
Description: "Im Folgenden werden Elemente und Attribute aufgeführt, mit denen Sie adaptive Kacheln erstellen können."
title: "Adaptive Kacheln – Schema und Vorlagen"
ms.assetid: 858FB05E-87A2-49CF-BE48-570980AD36C8
label: Adaptive tile schema and templates
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 08bdb46dba6fc93ada20b3fc585d3e24e29023a0

---
# Vorlagen für adaptive Kacheln: Schema und Richtlinien

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Im Folgenden werden Elemente und Attribute aufgeführt, mit denen Sie adaptive Kacheln erstellen können. Anweisungen und Beispiele finden Sie unter [Erstellen adaptiver Kacheln](tiles-and-notifications-create-adaptive-tiles.md).

## Kachel-Element


``` xml
<tile>
  
  <!-- Child elements -->
  visual
  
</tile>
```

## Visuelles Element


``` xml
<visual
  version? = integer
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string >
    
  <!-- Child elements -->
  binding+

</visual>
```

## Bindungselement


``` xml
<binding
  template = tileTemplateNameV3
  fallback? = tileTemplateNameV1
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string
  hint-textStacking? = "top" | "center" | "bottom"
  hint-overlay? = [0-100] >

  <!-- Child elements -->
  ( image
  | text
  | group
  )*

</binding>
```

## Bildelement


``` xml
<image
  src = string
  placement? = "inline" | "background" | "peek"
  alt? = string
  addImageQuery? = boolean
  hint-crop? = "none" | "circle"
  hint-removeMargin? = boolean
  hint-align? = "stretch" | "left" | "center" | "right" />
```

## Textelement


``` xml
<text
  lang? = string
  hint-style? = textStyle
  hint-wrap? = boolean
  hint-maxLines? = integer
  hint-minLines? = integer
  hint-align? = "left" | "center" | "right" >

  <!-- text goes here -->

</text>
```

textStyle values: caption captionSubtle body bodySubtle base baseSubtle subtitle subtitleSubtle title titleSubtle titleNumeral subheader subheaderSubtle subheaderNumeral header headerSubtle headerNumber

## Gruppenelement


``` xml
<group>

  <!-- Child elements -->
  subgroup+

</group>
```

## Untergruppenelement


``` xml
<subgroup
  hint-weight? = [0-100]
  hint-textStacking? = "top" | "center" | "bottom" >

  <!-- Child elements -->
  ( text
  | image
  )*

</subgroup>
```

## Verwandte Themen


* [Erstellen adaptiver Kacheln](tiles-and-notifications-create-adaptive-tiles.md)
 

 







<!--HONumber=Aug16_HO3-->


