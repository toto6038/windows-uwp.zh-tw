---
author: jwmsft
description: "了解 XAML 使用的空格處理規則。"
title: "XAML 與空格"
ms.assetid: 025F4A8E-9479-4668-8AFD-E20E7262DC24
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 88d4b155acb38a3ab11cc180d112fb3434af87a0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="xaml-and-whitespace"></a>XAML 與空格

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

了解 XAML 使用的空格處理規則。

## <a name="whitespace-processing"></a>空格處理

與 XML 一樣，XAML 中的空格字元為空格、換行字元及 Tab。 這些分別對應於 Unicode 值 0020、000A 以及 0009。 根據預設值，當 XAML 處理器處理在 XAML 檔案的元素之間發現的任何內部文字時，會執行下列空格正規化：

-   東亞字元之間的換行字元會被移除。
-   所有空格字元 (空格、換行字元及 Tab) 會轉換成空格。
-   所有連續的空格都會被刪除，並取代為一個空格。
-   在開始標記後的空格會被刪除。
-   在結束標記前的空格會被刪除。
-   「東亞字元」**會定義為 Unicode 字元範圍集：U+20000 到 U+2FFFD 與 U+30000 到 U+3FFFD。 這個子集有時也稱為「CJK 表意字元」**。 如需詳細資訊，請參閱 http://www.unicode.org。

"Default" 會對應 **xml:space** 屬性預設值代表的狀態。

### <a name="whitespace-in-inner-text-and-string-primitives"></a>內部文字的空格與字串基本類型

上述正規化規則適用於 XAML 元素中的內部文字。 在正規化後，XAML 處理器會將任何內部文字轉換成適當類型，如下所示：

-   如果屬性的類型不是集合，但也並非 **Object** 類型，XAML 處理器會嘗試使用其類型轉換器轉換成該類型。 如果轉換失敗，將導致 XAML 剖析錯誤。
-   如果屬性的類型是集合，而內部文字是連續的 (中間沒有元素標記)，內部文字會剖析為單一 **String**。 如果集合類型無法接受 **String**，也會導致 XAML 剖析器錯誤。
-   如果屬性的類型是 **Object**，內部文字會剖析為單一 **String**。 如果中間有標記元素，會導致 XAML 剖析器錯誤，因為 **Object** 類型意味著單一物件 (**String** 或其他)。
-   如果屬性的類型是集合，而且內部文字不是連續的，第一個子字串會轉換成 **String** 並新增為集合項目，中間的元素會新增為集合項目，而最後的結尾子字串 (若有的話) 會新增至集合做為第三個 **String** 項目。

### <a name="whitespace-and-text-content-models"></a>空格與文字內容模型

保留空格實際上只是為了所有可能的內容模型的子集考量。 該子集的內容模型由可以接受某些格式的單一 **String** 類型、專用的 **String** 集合，或是清單、集合或字典中的 **String** 與其他類型組合構成。

即使是可以接受字串的內容模型，這些內容模型內的預設行為是不會注意存在的任何空格。

### <a name="preserving-whitespace"></a>保留空格

有數種方法可以在來源 XAML 中保留空格，而不受 XAML 處理器空格正規化的影響。

`xml:space="preserve"`：在需要保留空格的元素層級指定此屬性。 請注意，它會保留所有空格，包括由程式碼編輯器或設計表面新增的空格，這是為了對齊標記元素以便在視覺上呈現直觀的巢狀結構。 這些空格的轉譯與包含元素的內容模型相關。 我們不建議您在根層級指定 `xml:space="preserve"`，因為大部分的物件模型不認為空格很重要。 較佳的做法是只在字串內部會呈現空格或需要空格的集合的元素層級具體設定該屬性。

實體與不分行空格：XAML 支援在文字物件模型內放置任何 Unicode 實體。 您可以使用專用實體，像是不分行空格 (UTF-8 編碼方式)。 您也可以使用支援不分行空格字元的 Rich Text 控制項。 如果您使用實體來模擬配置特性 (像是縮排)，由於實體的執行階段輸出將依據比一般的配置設施 (像是適當使用面板與邊界) 更多的因素而變更，因此應該謹慎使用。

