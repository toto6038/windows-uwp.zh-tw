---
description: 瞭解如何在 XAML 標記中使用 x:Null 標記延伸，以指定屬性的 Null 值。
title: xNull 標記延伸
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46256a5456583f9e434f76a2d06bc552c2cb0d3f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169032"
---
# <a name="xnull-markup-extension"></a>{x:Null} 標記延伸


在 XAML 標記中，指定屬性的 **null** 值。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>備註

**null** 是 C# 與 C++ 的 null 參考關鍵字。 Microsoft Visual Basicnull 的 null 參考關鍵字為 **Nothing**。

不同相依性屬性的初始預設值可能會不同，且不一定是 **null**。 此外，許多相依性屬性因為內部實作的緣故，不接受以 **null** 做為值 (無論是透過標記或程式碼)。 在這種情況下，以 **{x:Null}** 設定 XAML 屬性值可能導致發生剖析器例外狀況。

一些 Windows 執行階段類型，是可為 null 的類型。 在可為 null 的類型尚未使用 **null** 做為預設值的情況下，您可以使用 **{x:Null}**，在 XAML 中設為 **null** 值。 如果使用的是 Visual C++ 元件延伸 (C++/CX)，可為 null 的類型會以 [**Platform::IBox<T>**](/cpp/cppcx/platform-ibox-interface) 來表示。 如果使用的是 Microsoft .NET 語言，可為 null 的類型會以 [**Nullable<T>**](/dotnet/api/system.nullable-1) 來表示。

## <a name="related-topics"></a>相關主題

* [**空<T>**](/dotnet/api/system.nullable-1)
* [**IReference<T>**](/uwp/api/Windows.Foundation.IReference_T_)
 