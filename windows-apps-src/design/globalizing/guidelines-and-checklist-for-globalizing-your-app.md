---
Description: Design and develop your app in such a way that it functions appropriately on systems with different language and culture configurations.
Search.Refinement.TopicID: 180
title: 全球化指導方針
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: 2e2dc5186c028aa8f20c2cc1d697f1749b4f1765
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8930679"
---
# <a name="guidelines-for-globalization"></a>全球化指導方針

設計及開發您的應用程式，使其能夠在不同語言和文化設定的系統上正常運作。 使用 [**Globalization**](/uwp/api/Windows.Globalization?branch=live) API 格式化資料，避免在您的程式碼中假設語言、地區、字元分類、書寫系統、日期/時間格式、數字、貨幣、重量及排序規則。

| 建議 | 描述 |
| ------------- | ----------- |
| 在操縱和比較字串時將文化納入考量。 | 例如，不要在比較他們之前變更字串的大小寫。 請參閱[字串使用建議](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)。 |
| 在整理 (排序) 字串和其他資料時，不要假設該作業總是按照字母順序進行。 | 針對並非使用拉丁文書寫體的語言，整理是以像是發音或筆畫數等要素作為基礎的。 即使是使用拉丁文書寫體的語言，也不一定會依字母順序排序。 例如，在某些文化特性中，電話簿可能不是依字母順序排序。 Windows 可以為您處理排序，但是如果您建立自己的排序演算法，則務必將目標市場所使用的排序方法納入考量。 |
| 使用適當格式的數字、日期、時間、地址及電話號碼。 | 這些格式在不同文化、地區、語言和市場間都有所不同。 若要顯示這些資料，請使用 [**Globalization**](/uwp/api/Windows.Globalization?branch=live) API 取得適用於特定對象的適當格式。 請參閱[全球化您的日期/時間/數字格式](use-global-ready-formats.md)。 姓氏與名稱顯示的順序，以及地址的格式等也會有所不同。 請使用標準日期、時間及數字顯示。 請使用標準日期和時間選擇器控制項。 請使用標準地址資訊。 |
| 支援國際度量單位和貨幣。 | 即使最普遍的是公制系統和英制系統，但是，不同的國家或地區還是會使用不同的單位和比例。 如果您處理像是長度、溫度或面積等度量，請務必支援正確系統度量單位。 使用 [**GeographicRegion.CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse) 屬性取得在特定地區中使用的貨幣組。 |
| 使用 Unicode 進行字元編碼。 | 根據預設，Microsoft Visual Studio 會使用 Unicode 字元編碼所有文件。 如果您使用不同的編輯器，務必以適當的 Unicode 字元編碼儲存來源檔案。 所有 UWP API 都會傳回 UTF-16 編碼的字串。 |
| 支援國際紙張大小。 | 最常用的紙張大小會因為國家或地區而不同，所以，如果您包含根據紙張大小而定的功能 (例如，列印)，請確定可支援常用的國際紙張大小並加以測試。 |
| 記錄鍵盤或 IME 的語言。 | 當您的應用程式要求使用者輸入文字時，記錄目前啟用之鍵盤配置或輸入法編輯器 (IME) 的語言標記。 這可確保在稍後顯示輸入時，會以正確的格式顯示給使用者。 使用 [**Language.CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) 屬性取得目前的輸入語言。 |
| 請不要使用語言來假設使用者的地區；也請不要使用地區來假設使用者的語言。 | 語言和地區是不同的概念。 使用者可以使用語言的特殊地區變體 (例如，en-GB 是英國使用的英文，但是使用者可能在完全不同的國家或地區)。 考量您的應用程式是否需要關於使用者語言 (例如針對 UI 文字) 或地區 (例如針對授權問題) 的知識。 如需詳細資訊，請參閱[了解使用者設定檔語言和應用程式資訊清單語言](manage-language-and-region.md)。 |
| 比較語言標記的規則並非簡易規則。 | [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)很複雜。 在比較語言標記時會產生許多問題，包括比對指令碼資訊、傳統標記及多個地區變體的問題。 Windows 中的資源管理系統會為您處理比對工作。 您可以使用任何語言指定一組資源，系統就會為使用者和應用程式選擇適當的資源。 請參閱[應用程式資源和資源管理系統](../../app-resources/index.md)及[資源管理系統如何比對語言標記](../../app-resources/how-rms-matches-lang-tags.md)。 |
| 建議您將您的 UI 設計為可容納不同文字長度和文字大小的標籤和文字輸入控制項。 | 翻譯成不同語言的字串長度可能會大幅改變，因此您需要將您的 UI 控制項設為可根據其內容動態調整尺寸。 在其他語言中常見的字元包含位於英文中常用字母上方或下方的標記 (例如 Å 或 Ņ)。 使用標準字型大小和列高提供適當的垂直空間。 請注意，適用於其他語言的字型可能會需要較大的最小字型大小，以保持在可閱讀的情況。 請參閱 [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live) 命名空間中的類別。 |
| 支援閱讀順序的鏡像。 | 文字對齊和閱讀順序可以是由左至右 (例如，英文)，或由右至左 (RTL) (例如，阿拉伯文或希伯來文)。 如果您正在將產品當地語系化為使用和您自己的語言不同閱讀順序的語言，請確定 UI 元素的配置支援鏡像。 像是返回按鈕、UI 轉換效果及影像等項目都可能需要鏡像。 如需詳細資訊，請參閱[調整配置和字型及支援 RTL](adjust-layout-and-fonts--and-support-rtl.md)。 |
| 正確顯示文字和字型。 | 理想的字型、字型大小和文字的方向會根據不同市場而有所不同。 如需詳細資訊，請參閱[**調整配置和字型及支援 RTL**](adjust-layout-and-fonts--and-support-rtl.md) 和[國際字型](loc-international-fonts.md)。 |

## <a name="important-apis"></a>重要 API
 
* [Globalization](/uwp/api/Windows.Globalization?branch=live)
* [GeographicRegion.CurrenciesInUse](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language.CurrentInputMethodLanguageTag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>相關主題

* [字串使用建議](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [全球化您的日期/時間/數字格式](use-global-ready-formats.md)
* [了解使用者設定檔語言和應用程式資訊清單語言](manage-language-and-region.md)
* [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [應用程式資源和資源管理系統](../../app-resources/index.md)
* [資源管理系統如何比對語言標記](../../app-resources/how-rms-matches-lang-tags.md)
* [調整配置和字型及支援 RTL](adjust-layout-and-fonts--and-support-rtl.md)
* [國際字型](loc-international-fonts.md)
* [讓您的應用程式可當地語系化](prepare-your-app-for-localization.md)

## <a name="samples"></a>範例

* [全球化喜好設定範例](http://go.microsoft.com/fwlink/p/?linkid=231608)
