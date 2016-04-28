---
Description: Im Folgenden werden Elemente und Attribute aufgeführt, mit denen Sie adaptive Kacheln erstellen können.
title: Adaptive Kacheln – Schema und Vorlagen
ms.assetid: 858FB05E-87A2-49CF-BE48-570980AD36C8
label: Adaptive Kacheln – Schema und Vorlagen
template: detail.hbs
---

# Vorlagen für adaptive Kacheln: Schema und Richtlinien


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Im Folgenden werden Elemente und Attribute aufgeführt, mit denen Sie adaptive Kacheln erstellen können. Anweisungen und Beispiele finden Sie unter [Erstellen adaptiver Kacheln](tiles-and-notifications-create-adaptive-tiles.md).

## <span id="tile_element"></span><span id="TILE_ELEMENT"></span>Kachel-Element


``` syntax
<tile>
  
  <!-- Child elements -->
  visual
  
</tile>
```

## <span id="visual_element"></span><span id="VISUAL_ELEMENT"></span>Visuelles Element


``` syntax
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

## <span id="binding_element"></span><span id="BINDING_ELEMENT"></span>Bindungselement


``` syntax
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

## <span id="image_element"></span><span id="IMAGE_ELEMENT"></span>Bildelement


``` syntax
<image
  src = string
  placement? = "inline" | "background" | "peek"
  alt? = string
  addImageQuery? = boolean
  hint-crop? = "none" | "circle"
  hint-removeMargin? = boolean
  hint-align? = "stretch" | "left" | "center" | "right" />
```

## <span id="text_element"></span><span id="TEXT_ELEMENT"></span>Textelement


``` syntax
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

## <span id="group_element"></span><span id="GROUP_ELEMENT"></span>Gruppenelement


``` syntax
<group>

  <!-- Child elements -->
  subgroup+

</group>
```

## <span id="subgroup_element"></span><span id="SUBGROUP_ELEMENT"></span>Untergruppenelement


``` syntax
<subgroup
  hint-weight? = [0-100]
  hint-textStacking? = "top" | "center" | "bottom" >

  <!-- Child elements -->
  ( text
  | image
  )*

</subgroup>
```

**Hinweis**  
Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, hilft Ihnen die [archivierte Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132) weiter.

 

## <span id="related_topics"></span>Verwandte Themen


* [Erstellen adaptiver Kacheln](tiles-and-notifications-create-adaptive-tiles.md)
 

 






<!--HONumber=Mar16_HO1-->


