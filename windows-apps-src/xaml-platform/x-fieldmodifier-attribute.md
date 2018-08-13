---
author: jwmsft
description: 修改 XAML 編譯行為，以 public 存取而不是 private 預設行為來定義具名物件參考的欄位。
title: xFieldModifier 屬性
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: cad84be24836bc6a33a4ab08f1ca4fa2d9e97512
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.locfileid: "210486"
---
# <a name="xfieldmodifier-attribute"></a>x:FieldModifier 屬性

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

修改 XAML 編譯行為，以 **public** 存取而不是 **private** 預設行為來定義具名物件參考的欄位。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>相依性

相同元素上也必須提供 [x:Name 屬性](x-name-attribute.md)。

## <a name="remarks"></a>備註

**x:FieldModifier** 屬性的值會依程式設計語言而有所不同。 有效值為 **private**、**public**、**protected**、**internal** 或 **friend**。 以 C#、Microsoft Visual Basic 或 Visual C++ 元件延伸 (C++/CX) 來說，您可以提供字串值 "public" 或 "Public"；剖析器對這個屬性值並沒有強制大小寫。

**Private** 存取是預設值。

**x:FieldModifier** 只與具有 [x:Name 屬性](x-name-attribute.md)的元素相關，因為該名稱會在欄位是公用時用來參考該欄位。

**注意** Windows 執行階段 XAML 不支援 **x:ClassModifier** 或 **x:Subclass**。

