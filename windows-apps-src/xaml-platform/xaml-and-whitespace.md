---
description: 瞭解 XAML 用來處理空白字元空間、換行字元和定位字元的規則。
title: XAML 與空格
ms.assetid: 025F4A8E-9479-4668-8AFD-E20E7262DC24
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 16d5b0b3faa4d356ced2eb352192bd1a91882475
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094625"
---
# <a name="xaml-and-whitespace"></a>XAML 與空格


了解 XAML 使用的空格處理規則。

## <a name="whitespace-processing"></a>空格處理

XAML 中的空白字元是空格、換行字元和 tab 鍵，與 XML 一致。這些會分別對應至 Unicode 值0020、000A 和0009。 根據預設值，當 XAML 處理器處理在 XAML 檔案的元素之間發現的任何內部文字時，會執行下列空格正規化：

-   東亞字元間的換行字元會遭到移除。
-   所有空白字元 (空格、換行字元、定位字元) 都會轉換成空格。
-   所有連續的空格會被刪除並取代為一個空格。
-   緊接在開始標記之後的空格會遭到刪除。
-   緊接在結束標記之前的空格會遭到刪除。
-   「東亞字元」** 會定義為 Unicode 字元範圍集：U+20000 到 U+2FFFD 與 U+30000 到 U+3FFFD。 這個子集有時也稱為 *CJK 表意文字*。 如需詳細資訊，請參閱http://www.unicode.org。

「預設」會對應到 **xml:space** 屬性的預設值所表示的狀態。

### <a name="whitespace-in-inner-text-and-string-primitives"></a>內部文字的空格與字串基本類型

上述正規化規則適用於 XAML 元素中的內部文字。 在正規化後，XAML 處理器會將任何內部文字轉換成適當類型，如下所示：

-   如果屬性的類型不是集合，但也並非 **Object** 類型，XAML 處理器會嘗試使用其類型轉換器轉換成該類型。 如果轉換失敗，將導致 XAML 剖析錯誤。
-   如果屬性的類型是集合，而內部文字是連續的 (中間沒有元素標記)，內部文字會剖析為單一 **String**。 如果集合類型無法接受 **String**，也會導致 XAML 剖析器錯誤。
-   如果屬性的類型是 **Object**，內部文字會剖析為單一 **String**。 如果中間有標記元素，會導致 XAML 剖析器錯誤，因為 **Object** 類型意味著單一物件 (**String** 或其他)。
-   如果屬性的類型是集合，而且內部文字不是連續的，第一個子字串會轉換成 **String** 並新增為集合項目，中間的元素會新增為集合項目，而最後的結尾子字串 (若有的話) 會新增至集合做為第三個 **String** 項目。

### <a name="whitespace-and-text-content-models"></a>空格與文字內容模型

保留空格實際上只是為了所有可能的內容模型的子集考量。 該子集的內容模型由可以接受某些格式的單一 **String** 類型、專用的 **String** 集合，或是清單、集合或字典中的 **String** 與其他類型組合構成。

即使是可接受字串的內容模型，這些內容模型中的預設行為也都是不會將任何保留的空白字元視為必要。

### <a name="preserving-whitespace"></a>保留空格

有數種方法可以在來源 XAML 中保留空格，而不受 XAML 處理器空格正規化的影響。

`xml:space="preserve"`：在需要保留空格的元素層級指定此屬性。 請注意，它會保留所有空格，包括由程式碼編輯器或設計表面新增的空格，這是為了對齊標記元素以便在視覺上呈現直觀的巢狀結構。 這些空格的轉譯與包含元素的內容模型相關。 我們不建議您在根層級指定 `xml:space="preserve"`，因為大部分的物件模型不認為空格很重要。 比較好的做法是，只有當項目會呈現字串內的空白字元或本身是需要空白字元的集合時，才在項目層級特別設定這個屬性。

實體與不分行空格：XAML 支援在文字物件模型內放置任何 Unicode 實體。 您可以使用專用實體，像是不分行空格 (UTF-8 編碼方式)。 您也可以使用支援不分行空格字元的 RTF 文字控制項。 如果您使用實體來模擬配置特性 (像是縮排)，由於實體的執行階段輸出將依據比一般的配置設施 (像是適當使用面板與邊界) 更多的因素而變更，因此應該謹慎使用。

