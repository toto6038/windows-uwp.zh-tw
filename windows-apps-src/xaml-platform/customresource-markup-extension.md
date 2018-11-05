---
author: jwmsft
description: 透過評估源自於自訂資源查詢實作的資源參考，為所有 XAML 屬性提供一個值。 資源查詢由 CustomXamlResourceLoader 類別實作執行。
title: CustomResource 標記延伸
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6fb88fc3a764a89dfe7ac8a9c399149b6b98eca5
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6035394"
---
# <a name="customresource-markup-extension"></a>{CustomResource} 標記延伸


透過評估源自於自訂資源查詢實作的資源參考，為所有 XAML 屬性提供一個值。 資源查詢由 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 類別實作執行。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 詞彙 | 說明 |
|------|-------------|
| 索引鍵 | 要求的資源的索引鍵。 最初指派索引鍵的方式取決於目前已登錄使用的 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 類別實作。 |

## <a name="remarks"></a>備註

**CustomResource** 是取得定義在自訂資源存放庫其他位置的值的一種方法。 這項技術相對來說較為先進，而大多數的 Windows 執行階段應用程式案例並未使用這項技術。

這個主題不會描述 **CustomResource** 如何解析為資源字典，原因是這些方法會因為實作 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 的方式而有很大的差異。

Windows 執行階段 XAML 剖析器只要在標記中遇到使用 `{CustomResource}` 時，就會呼叫 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 實作的 [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) 方法。 傳遞到 **GetResource** 的 *resourceId* 來自於 *key* 引數，而其他輸入參數則來自於內容，例如用法所套用的屬性。

`{CustomResource}` 用法預設無法運作 ([**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) 的基底實作不完整)。 為了讓 `{CustomResource}` 參考有效，您必須執行下列每個步驟：

1.  從 [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) 衍生自訂類別並覆寫 [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) 方法。 不要在實作中呼叫基底。
2.  設定 [**CustomXamlResourceLoader.Current**](https://msdn.microsoft.com/library/windows/apps/br243328) 使其參考初始化邏輯中的類別。 這必須在載入任何包含 `{CustomResource}` 延伸用法的頁面層級 XAML 之前完成。 其中一個設定 **CustomXamlResourceLoader.Current** 的位置，就是在 App.xaml 程式碼後置範本中為您產生的 [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 子類別建構函式。
3.  現在您可以在應用程式載入為頁面的 XAML 中使用 `{CustomResource}` 延伸，或是從 XAML 資源字典內使用。

**CustomResource** 是一個標記延伸。 當有需要將屬性值逸出文字值或處理常式名稱時，通常就會實作標記延伸，而且這需求是全域性的，而不只是在特定類型或屬性放置類型轉換器。 XAML 的所有標記延伸會在屬性語法中使用 "\{" 和 "\}" 字元，這是慣例，XAML 處理器藉此來辨識必須處理屬性的標記延伸。

## <a name="related-topics"></a>相關主題

* [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327)
* [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340)

