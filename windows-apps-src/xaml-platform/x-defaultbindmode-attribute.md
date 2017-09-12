---
author: tbd
description: "在 XAML 標記中，指定 x:Bind 的預設模式"
title: "xDefaultBindMode 標記延伸"
ms.assetid: 
ms.author: 
ms.date: 
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: 0fa037b4c59566cb1b9bacd4d2e36520a86c508d
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2017
---
# <a name="xdefaultbindmode-markup-extension"></a>{x:DefaultBindMode} 標記延伸

\[ 已為 Windows 10 上的 UWP 應用程式更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在 XAML 標記中，指定 x:Bind 的預設模式。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>備註

x:Bind 的預設模式為 OneTime，這是基於效能考量所選擇，因為使用 OneTime 將會導致產生更多程式碼來連結和處理變更偵測。 **x:DefaultBindMode** 可用來變更標記樹狀結構特定片段的 x:Bind 預設模式。 選取的模式將會在該元素及其子系上，套用任何未明確指定模式做為繫結之一部分的 x:Bind 運算式。

## <a name="related-topics"></a>相關主題

* [**x:Bind**](https://docs.microsoft.com/en-us/windows/uwp/xaml-platform/x-bind-markup-extension)
