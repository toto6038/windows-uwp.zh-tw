---
author: stevewhims
Description: Design your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: 調整配置和字型及支援 RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 可當地語系化性, 當地語系化, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: cf3a2d781dc916fbda9a9d6386dee4e2e6144873
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "1673975"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>調整配置和字型及支援 RTL

設計您的應用程式以支援多種語言的配置和字型，包括 RTL (從右至左 ) 文字方向。 流程方向是文字書寫和顯示，以及使用者的眼睛掃過頁面上 UI 元素的方向。

## <a name="layout-guidelines"></a>配置指導方針

德文和芬蘭文等語言通常會使用比英文更多的字元。 遠東字型通常需要更多高度。 而像是阿拉伯文和希伯來文等語言，則要求配置面板和文字元素必須採用由右至左 (RTL) 的閱讀順序。

因為翻譯文字的長度會產生變化，建議您使用動態 UI 配置機制，而非絕對位置、固定寬度或固定高度。 虛擬當地語系化您的應用程式，可發現任何有問題的邊緣案例，其中您的 UI 元素可能無法根據內容正常調整尺寸。

針對 RTL 語言，請使用 [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 屬性，然後設定對稱邊框間距和邊界。 配置面板，例如 [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) 會自動使用您設定的 **FlowDirection** 縮放及翻轉。

以下是如何在您的應用程式中將 FlowDirection 屬性作為負責當地語系化的人員可適當設定的資源公開的範例。

首先，在您應用程式的資源檔 (.resw) 中，使用名稱「MainPage.FlowDirection」(除了「MainPage」之外，您也可以使用任何您想要的名稱) 新增屬性識別碼。

然後，使用 **x:Uid** 來將您的主要 **Page** 元素與該屬性識別碼產生關聯。

```xaml
<Page x:Uid="MainPage">
```

如需關於資源檔 (.resw)、屬性識別碼，和 **x:Uid** 的詳細資訊，請參閱[當地語系化您 UI 及應用程式封裝資訊清單中的字串](../../app-resources/localize-strings-ui-manifest.md)。

建議您避免在任何 UI 元素上，根據語言設定絕對配置值。 但如果絕對無法避免，您可以以「TitleText.Width」的形式建立屬性識別碼。

```xaml
<TextBlock x:Uid="TitleText">
```

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

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>處理由右至左 (RTL) 語言的最佳做法

當您的應用程式要針對由右至左 (RTL) 語言進行當地語系化時，可以使用 API 來設定您頁面根配置面板的預設文字方向。 這會讓包含在根面板中的所有控制項適當地回應預設文字方向。 支援多種語言時，請使用最優先應用程式執行階段語言的 `LayoutDirection` 來設定 [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 屬性 (請參閱以下程式碼範例)。 Windows 中包含的大部分控制項均已使用 **FlowDirection**。 如果您要實作自訂控制項，則這類控制項應使用 **FlowDirection**，以針對 RTL 及 LTR 語言配置進行適當地變更。

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

### <a name="rtl-faq"></a>RTL 常見問題集 

**問：****FlowDirection** 是否會根據目前所選取的語言自動設定？ 例如，如果我選取英文，它將會從左至右顯示，而如果我選取阿拉伯文，它是否會從右至左顯示？

> **答：****FlowDirection** 不會將語言納入考量。 您要針對目前所顯示的語言適當地設定 **FlowDirection**。 請參閱上述範例程式碼。

**問︰** 我不太熟悉當地語系化。 資源是否已經包含文字方向？ 是否可能從目前的語言判斷文字方向？

> **答︰** 如果您使用目前的最佳做法，則資源並未直接包含文字方向。 您必須判斷目前語言的文字方向。 以下有兩種做法。
> 
> 最好的方法是使用最慣用語言的 **LayoutDirection** 來設定 RootFrame 的 **FlowDirection** 屬性。 RootFrame 中的所有控制項都會從 RootFrame 繼承 FlowDirection。
> 
> 另一種方法是在您要進行當地語系化的 RTL 語言資源檔 (.resw 檔案) 中設定 FlowDirection。 例如，您可能會有阿拉伯文的 resw 檔案及希伯來文的 resw 檔案。 您可以在這些檔案中使用 x:UID 來設定 FlowDirection。 但這個方法會比程式設計方法更容易發生錯誤。

## <a name="important-apis"></a>重要 API

* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>相關主題

* [當地語系化 UI 及應用程式套件資訊清單中的字串](../../app-resources/localize-strings-ui-manifest.md)
* [針對語言、縮放比例及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [了解使用者設定檔語言和應用程式資訊清單語言](manage-language-and-region.md)