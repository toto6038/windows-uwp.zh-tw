---
description: 修改 XAML 編譯行為，以 public 存取而不是 private 預設行為來定義具名物件參考的欄位。
title: xFieldModifier 屬性
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 751cda36fc58d0e6add9204327a74ec947c9fc53
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8485972"
---
# <a name="xfieldmodifier-attribute"></a>x:FieldModifier 屬性


修改 XAML 編譯行為，以 **public** 存取而不是 **private** 預設行為來定義具名物件參考的欄位。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>相依性

相同元素上也必須提供 [x:Name 屬性](x-name-attribute.md)。

## <a name="remarks"></a>備註

**x:FieldModifier** 屬性的值會依程式設計語言而有所不同。 有效值為 **private**、**public**、**protected**、**internal** 或 **friend**。 C#、 Microsoft Visual Basic 或 VisualC + + 元件延伸 (C + + /CX)，您可以提供字串值"public"Public";在剖析器不會強制執行對這個屬性值的案例。

**Private** 存取是預設值。

**x:FieldModifier** 只與具有 [x:Name 屬性](x-name-attribute.md)的元素相關，因為該名稱會在欄位是公用時用來參考該欄位。

**注意：** **X:classmodifier**或**X:subclass**，Windows 執行階段 XAML 不支援。

