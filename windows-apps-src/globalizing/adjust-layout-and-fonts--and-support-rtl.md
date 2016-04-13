---
Description: 開發您的 app 以支援多種語言的配置和字型，包括 RTL (從右至左 ) 文字方向。
title: 調整配置和字型並支援 RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
---

# 調整配置和字型並支援 RTL


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


開發您的 app 以支援多種語言的配置和字型，包括 RTL (從右至左 ) 文字方向。

## <span id="Layout_guidelines"> </span> <span id="layout_guidelines"> </span> <span id="LAYOUT_GUIDELINES"> </span>配置指導方針


有些語言 (例如德文和芬蘭文) 的文字所需的空間比英文更多。 有些語言的字型 (例如日文) 需要較高的高度。 而有些語言 (例如阿拉伯文和希伯來文) 則要求文字配置和應用程式配置必須是採用從右至左 (RTL) 的閱讀順序。

請使用彈性配置機制，而不要使用絕對定位、固定寬度或固定高度。 必要時，可根據語言調整特定 UI 元素。

### <span id="XAML"> </span> <span id="xaml"> </span>XAML

為元素指定 **Uid**：

```XAML
<TextBlock x:Uid="Block1">
```

確保應用程式的 ResW 檔案有 Block1.Width 的資源，您可以針對要當地語系化的每個語言設定這個資源。

對於使用 C++、C\# 或 Visual Basic 的 Windows 市集應用程式，請使用 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 屬性以及對稱式邊框間距和邊界，以當地語系化其他配置方向。

XAML 配置控制項 (例如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)) 會使用 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 屬性自動縮放和翻轉。 在應用程式中公開您自己的 **FlowDirection** 屬性以做為當地語系化人員的資源。

為應用程式的主頁面指定 **Uid**：

```XAML
<Page x:Uid="MainPage">
```

確保應用程式的 **ResW** 檔案有 MainPage.FlowDirection 的資源，您可以針對要當地語系化的每個語言設定這個資源。

### <span id="HTML"> </span> <span id="html"> </span>HTML

針對使用 JavaScript 的 Windows 市集應用程式，請使用[階層式樣式表 (CSS)](https://msdn.microsoft.com/library/ms531209) 配置機制，例如 [-ms-grid](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#g_section) 和 [–ms-box](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#f_section)。 請使用對稱式邊框間距和邊界，以便能夠為不同的配置方向進行當地語系化。

您的應用程式也可以使用 [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) 虛擬類別選取器，根據應用程式的語言來調整 CSS 屬性，例如特定元素的寬度。 為了啟用，主控處理程序會將根元素的 **lang** 屬性設定為應用程式語言。

**CSS**
```CSS
.item:-ms-lang(de, fi) { width: 350px; }
```

使用 JavaScript 的「Windows 市集」應用程式如果使用 ui-light.css 或 ui-dark.css 樣式表，就會根據應用程式語言自動設定其主體配置方向。 下列 CSS 位於 ui-light 和 ui-dark.css 中，您不需要自行撰寫這個 CSS。

**CSS**
```CSS
body:-ms-lang(ar,he…) { direction: rtl;}
```

這表示大部分的應用程式配置在系統使用從右至左的語言時都能正確設定。

就像 [WinJS.UI](https://msdn.microsoft.com/library/windows/apps/br229782) 控制項，您的應用程式可以使用 [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) 虛擬類別選取器調整實體 CSS 屬性，例如 **margin** 和 **padding**。 您不需要調整使用 **after** 和 **before** 關鍵字的邏輯 CSS 屬性。

不要在 HTML 中使用 **align** 屬性 (Property) 或屬性 (Attribute)。 請改為使用 **direction** 屬性來控制特定元件的對齊。

使用 [**writing-mode**](https://msdn.microsoft.com/library/ms531187) 屬性支援 CSS 中的垂直文字配置。

## <span id="Mirroring_images"> </span> <span id="mirroring_images"> </span> <span id="MIRRORING_IMAGES"> </span>鏡像影像


### <span id="XAML"> </span> <span id="xaml"> </span>XAML

如果應用程式含有必須針對 RTL 進行鏡像的影像 (也就是相同的影像可以翻轉)，您可以套用 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 屬性：

```XAML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

### <span id="HTML"> </span> <span id="html"> </span>HTML

如果應用程式含有必須針對 RTL 進行鏡像的影像 (也就是相同的影像可以翻轉)，您可以使用 CSS 轉換，藉由將 .mirrorable 類別新增到您的元素並新增下列 CSS 類別，在轉譯時進行影像鏡像：

```CSS
.mirrorable { transform: scaleX(-1); }
```

**針對 XAML 與 HTML：**如果應用程式需要不同的影像才能正確翻轉影像，您可以使用資源管理系統搭配 [layoutdir 限定詞](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。 系統會在[應用程式語言](manage-language-and-region.md)設定為 RTL 語言時，選擇名為 file.layoutdir-rtl.png 的影像。 在已翻轉影像的某些部分，但其他部分尚未翻轉時，可能需要這個處理方式。

## <span id="Fonts"> </span> <span id="fonts"> </span> <span id="FONTS"> </span>字型


**針對 XAML 與 HTML：**請使用 [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) 字型對應 API，以程式設計方式存取特定語言的建議字型系列、大小、粗細及樣式。 **LanguageFont** 物件會針對各種內容類別 (包括 UI 標頭、通知、內文，以及使用者可以編輯的文件主體字型)，提供正確字型資訊的存取權。

### <span id="HTML"> </span> <span id="html"> </span>HTML

使用 JavaScript 的 Windows 市集應用程式如果使用 ui-light.css 或 ui-dark.css 樣式表，會根據應用程式語言將字型自動設定為最適當的字型。 主控處理程序會將根元素的 **lang** 屬性設定為應用程式語言。

在單一頁面上顯示多個語言的應用程式應該在每個語言設定區段的 **lang** 屬性。 [
            **:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) 虛擬類別選取器會針對頁面的每個區段挑選正確的字型。

 

 





<!--HONumber=Mar16_HO4-->


