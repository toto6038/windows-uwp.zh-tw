---
description: 透過評估源自於自訂資源查詢實作的資源參考，為所有 XAML 屬性提供一個值。 資源查詢由 CustomXamlResourceLoader 類別實作執行。
title: CustomResource 標記延伸
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 094a2a957493787acef8e97b49b61778fc1ec42f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155102"
---
# <a name="customresource-markup-extension"></a>{CustomResource} 標記延伸


透過評估源自於自訂資源查詢實作的資源參考，為所有 XAML 屬性提供一個值。 資源查閱是由 [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 類別的實作為執行。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 詞彙 | 描述 |
|------|-------------|
| 索引鍵 | 要求資源的金鑰。 最初指派索引鍵的方式取決於目前已登錄使用的 [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 類別實作。 |

## <a name="remarks"></a>備註

**CustomResource** 是取得定義在自訂資源存放庫其他位置的值的一種方法。 這項技術相對來說較為先進，而大多數的 Windows 執行階段應用程式案例並未使用這項技術。

這個主題不會描述 **CustomResource** 如何解析為資源字典，原因是這些方法會因為實作 [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 的方式而有很大的差異。

Windows 執行階段 XAML 剖析器只要在標記中遇到使用 `{CustomResource}` 時，就會呼叫 [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 實作的 [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 方法。 傳遞到 **GetResource** 的 *resourceId* 來自於 *key* 引數，而其他輸入參數則來自於內容，例如用法所套用的屬性。

`{CustomResource}` 用法預設無法運作 ([**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 的基底實作不完整)。 為了讓 `{CustomResource}` 參考有效，您必須執行下列每個步驟：

1.  從 [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 衍生自訂類別並覆寫 [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 方法。 不要在實作中呼叫基底。
2.  設定 [**CustomXamlResourceLoader.Current**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.current) 使其參考初始化邏輯中的類別。 這必須在載入任何包含 `{CustomResource}` 延伸用法的頁面層級 XAML 之前完成。 其中一個設定 **CustomXamlResourceLoader.Current** 的位置，就是在 App.xaml 程式碼後置範本中為您產生的 [**Application**](/uwp/api/Windows.UI.Xaml.Application) 子類別建構函式。
3.  現在您可以在應用程式載入為頁面的 XAML 中使用 `{CustomResource}` 延伸，或是從 XAML 資源字典內使用。

**CustomResource** 是一個標記延伸。 如果必須將屬性 (Attribute) 值加上逸出符號，以免成為常值或處理常式名稱，而且這個動作必須更全面地實施 (而不是只對特定類型或屬性 (Property) 設定類型轉換子 (Type Converter))，則通常會實作標記延伸。 XAML 中的所有標記延伸 \{ \} 都會在其屬性語法中使用 "" 和 "" 字元，這是 xaml 處理器辨識標記延伸必須處理屬性的慣例。

## <a name="related-topics"></a>相關主題

* [ResourceDictionary 與 XAML 資源參考](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)
* [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)