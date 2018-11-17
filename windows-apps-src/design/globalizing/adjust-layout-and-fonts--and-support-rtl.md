---
author: stevewhims
Description: Design your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: 調整配置和字型及支援 RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: stwhi
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, 可當地語系化性, 當地語系化, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: ac69701eca128d327dfd80cfddc7fad41c50c50e
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7149608"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>調整配置和字型及支援 RTL
設計您的應用程式以支援多種語言的配置和字型，包括 RTL (從右至左 ) 文字方向。 流程方向是文字書寫和顯示，以及使用者的眼睛掃過頁面上 UI 元素的方向。

## <a name="layout-guidelines"></a>配置指導方針
德文和芬蘭文等語言通常會使用比英文更多的字元。 遠東字型通常需要更多高度。 而像是阿拉伯文和希伯來文等語言，則要求配置面板和文字元素必須採用由右至左 (RTL) 的閱讀順序。

因為翻譯文字計量中的這些變化，我們建議您的使用者介面 (UI) 不要採用絕對位置、固定寬度，或固定高度。 相反地，利用 Windows UI 項目中內建的動態配置機制。 例如，內容控制項 (例如按鈕)、項目控制項 (例如方格檢視和清單檢視) 和配置面板 (例如方格和 Stackpanel) 預設會自動調整大小和重新排列以符合其內容。 虛擬當地語系化您的應用程式，可發現任何有問題的邊緣案例，其中您的 UI 元素可能無法根據內容正常調整尺寸。

動態配置是建議的技術，而您可以在大部分的案例使用它。 較不建議但仍然更勝於將尺寸內建到您的 UI 標記的是將配置值設定為語言的函式。 以下是如何在您的 App 中將 Width 屬性作為負責當地語系化的人員可按照語言適當設定的資源公開的範例。 首先，在您 App 的資源檔 (.resw) 中，使用名稱「TitleText.Width」(除了「TitleText」之外，您也可以使用任何您想要的名稱) 新增屬性識別碼。 然後，使用 **x:Uid** 來將一或多個 UI 元素與此屬性識別碼產生關聯。

```xaml
<TextBlock x:Uid="TitleText">
```

如需關於資源檔 (.resw)、屬性識別碼，和 **x:Uid** 的詳細資訊，請參閱[當地語系化 UI 及應用程式封裝資訊清單中的字串](../../app-resources/localize-strings-ui-manifest.md)。

## <a name="fonts"></a>字型
您可以使用 [**LanguageFont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live) 字型對應類別，以程式設計方式存取特定語言的建議字型系列、大小、粗細及樣式。 **LanguageFont** 類別會針對各種內容類別 (包括 UI 標頭、通知、內文，以及使用者可以編輯的文件內文字型)，提供正確字型資訊的存取權。

## <a name="mirroring-images"></a>鏡像影像
如果應用程式含有必須針對 RTL 進行鏡像的影像 (也就是相同的影像可以翻轉)，您可以使用 **FlowDirection**。

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

如果您的應用程式需要不同的影像才能正確翻轉影像，您可以搭配 `LayoutDirection` 限定詞使用資源管理系統 (請參閱[針對語言、縮放比例及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)中的＜LayoutDirection＞一節)。 系統會在應用程式執行階段語言 (請參閱[了解使用者設定檔語言和應用程式資訊清單語言](manage-language-and-region.md)) 設為 RTL 語言時選擇名為 `file.layoutdir-rtl.png` 的影像。 在已翻轉影像的某些部分，但其他部分尚未翻轉時，可能需要這個處理方式。

## <a name="handling-right-to-left-rtl-languages"></a>處理從右至左 (RTL) 的語言
當您的 App 當地語系化為從右至左 (RTL) 的語言，請使用 [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 屬性，然後設定對稱邊框間距和邊界。 配置面板，例如 [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) 會自動使用您設定的 **FlowDirection** 值縮放及翻轉。

在您的頁面的根配置面板 (或框架) 或頁面本身上設定 **FlowDirection**。 這會致使其中包含的所有控制項繼承該屬性。

> [!IMPORTANT]
> 不過，**FlowDirection** *不是*根據 Windows 設定中選取的顯示語言自動設定；其也不會動態變更以回應使用者切換顯示語言。 如果使用者將 Windows 設定從英文切換為阿拉伯文，例如，則 **FlowDirection** 屬性將*不會*自動從左至右變更為從右至左。 身為 App 開發人員，您要針對目前所顯示的語言適當地設定 **FlowDirection**。

程式設計的技術是使用慣用的使用者顯示語言的 `LayoutDirection` 屬性，來設定 [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 屬性 (請參見下列程式碼範例)。 Windows 中包含的大部分控制項均已使用 **FlowDirection**。 如果您要實作自訂控制項，則應使用 **FlowDirection**，以針對 RTL 及 LTR 語言配置進行適當的變更。

```csharp    
this.languageTag = Windows.Globalization.ApplicationLanguages.Languages[0];

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

var flowDirectionSetting = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
if (flowDirectionSetting == "LTR")
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.LeftToRight;
}
else
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.RightToLeft;
}
```

```cppwinrt
#include <winrt/Windows.ApplicationModel.Resources.Core.h>
#include <winrt/Windows.Globalization.h>
...

m_languageTag = Windows::Globalization::ApplicationLanguages::Languages().GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView().QualifierValues().Lookup(L"LayoutDirection");
if (flowDirectionSetting == L"LTR")
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::LeftToRight);
}
else
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::RightToLeft);
}
```

```cpp
this->languageTag = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView()->QualifierValues->Lookup("LayoutDirection");
if (flowDirectionSetting == "LTR")
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::LeftToRight;
}
else
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::RightToLeft;
}
```

以上技術會使 **FlowDirection** 成為慣用的使用者顯示語言的 `LayoutDirection` 屬性的函式。 如果有任何原因，您不想要該邏輯，則您可以在您的 App 中公開 FlowDirection 屬性，做為當地語系化人員的資源，可適當地針對他們翻譯的各個目標語言進行設定。

首先，在您 App 的資源檔 (.resw) 中，使用名稱「MainPage.FlowDirection」(除了「MainPage」之外，您也可以使用任何您想要的名稱) 新增屬性識別碼。 然後，使用 **x:Uid** 來將您的主要 **Page** 元素 (或其根配置面板或框架) 與此屬性識別碼產生關聯。

```xaml
<Page x:Uid="MainPage">
```

除了所有語言的單行程式碼以外，這也取決於翻譯人員針對每個翻譯的語言是否正確「翻譯」此屬性資源；所以當您使用這項技術時，請留意發生人為錯誤的額外機會。

## <a name="important-apis"></a>重要 API
* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>相關主題
* [當地語系化 UI 及應用程式套件資訊清單中的字串](../../app-resources/localize-strings-ui-manifest.md)
* [針對語言、縮放比例及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [了解使用者設定檔語言和應用程式資訊清單語言](manage-language-and-region.md)