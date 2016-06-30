---
author: mijacobs
Description: "以下是用來建立彈性磚的元素和屬性。"
title: "彈性磚結構描述與範本"
ms.assetid: 858FB05E-87A2-49CF-BE48-570980AD36C8
label: Adaptive tile schema and templates
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: a5d061515eee1ab64f17e4f5aab8846adbd1c8f1

---

# 彈性磚範本：結構描述和指導方針

以下是用來建立彈性磚的元素和屬性。 如需相關指示與範例，請參閱[建立彈性磚](tiles-and-notifications-create-adaptive-tiles.md)。

## <span id="tile_element"></span><span id="TILE_ELEMENT"></span>磚元素


``` syntax
<tile>
  
  <!-- Child elements -->
  visual
  
</tile>
```

## <span id="visual_element"></span><span id="VISUAL_ELEMENT"></span>視覺元素


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

## <span id="binding_element"></span><span id="BINDING_ELEMENT"></span>正在繫結元素


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

## <span id="image_element"></span><span id="IMAGE_ELEMENT"></span>影像元素


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

## <span id="text_element"></span><span id="TEXT_ELEMENT"></span>文字元素


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

textStyle 值：輔助字幕 captionSubtle 內文 bodySubtle 基底 baseSubtle 字幕 subtitleSubtle 標題 titleSubtle titleNumeral 次標題 subheaderSubtle subheaderNumeral 標題 headerSubtle headerNumber

## <span id="group_element"></span><span id="GROUP_ELEMENT"></span>群組元素


``` syntax
<group>

  <!-- Child elements -->
  subgroup+

</group>
```

## <span id="subgroup_element"></span><span id="SUBGROUP_ELEMENT"></span>子群組元素


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

## <span id="related_topics"></span>相關主題


* [建立彈性磚](tiles-and-notifications-create-adaptive-tiles.md)
 

 







<!--HONumber=Jun16_HO4-->


