---
description: 在 XAML 標記中，為屬性指定 null 值。
title: x:Null 標記延伸
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0589249c65d301e3e74b305b92842fe4d3f61929
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372281"
---
# <a name="xnull-markup-extension"></a>{x:Null} 標記延伸


在 XAML 標記中，為屬性指定 **null** 值。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>備註

**null** 是 C# 與 C++ 的 null 參考關鍵字。 Microsoft Visual Basicnull 的 null 參考關鍵字為 **Nothing**。

不同相依性屬性的初始預設值可能會不同，且不一定是 **null**。 此外，許多相依性屬性因為內部實作的緣故，不接受以 **null** 做為值 (無論是透過標記或程式碼)。 在這種情況下，以 **{x:Null}** 設定 XAML 屬性值可能導致發生剖析器例外狀況。

一些 Windows 執行階段類型，是可為 null 的類型。 在可為 null 的類型尚未使用 **null** 做為預設值的情況下，您可以使用 **{x:Null}** ，在 XAML 中設為 **null** 值。 如果使用視覺效果C++元件擴充功能 (C++/CX)，可為 null 的類型以[ **platform:: ibox<T>** ](https://docs.microsoft.com/cpp/cppcx/platform-ibox-interface)。 如果使用的是 Microsoft .NET 語言，可為 null 的類型會以 [**Nullable<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1?redirectedfrom=MSDN) 來表示。

## <a name="related-topics"></a>相關主題

* [**Nullable<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1?redirectedfrom=MSDN)
* [**IReference<T>** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IReference_T_)
 

