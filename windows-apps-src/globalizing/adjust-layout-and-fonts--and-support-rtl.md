---
author: DelfCo
Description: "開發您的 app 以支援多種語言的配置和字型，包括 RTL (從右至左 ) 文字方向。"
title: "調整配置和字型並支援 RTL"
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 9c700928d2ec0da21b518528289034296637eeff
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>調整配置和字型並支援 RTL
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

開發您的應用程式以支援多種語言的配置和字型，包括 RTL (從右至左 ) 文字方向。

## <a name="layout-guidelines"></a>配置指導方針


有些語言 (例如德文和芬蘭文) 的文字需要比英文更多的空間。 有些語言的字型 (例如日文) 需要較高的高度。 而有些語言 (例如阿拉伯文和希伯來文) 則要求文字配置和應用程式配置必須是採用從右至左 (RTL) 的閱讀順序。

請使用彈性配置機制，而不要使用絕對定位、固定寬度或固定高度。 必要時，可根據語言調整特定 UI 元素。

為元素指定 **Uid**：

```XML
<TextBlock x:Uid="Block1">
```

確保應用程式的 ResW 檔案有 Block1.Width 的資源，您可以針對要當地語系化的每個語言設定這個資源。

對於使用 C++、C\# 或 Visual Basic 的 Windows 市集應用程式，請使用 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 屬性以及對稱式邊框間距和邊界，以當地語系化其他配置方向。

XAML 配置控制項 (例如 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)) 會使用 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 屬性自動縮放和翻轉。 在應用程式中公開您自己的 **FlowDirection** 屬性以做為當地語系化人員的資源。

為應用程式的主頁面指定 **Uid**：

```XML
<Page x:Uid="MainPage">
```

確保應用程式的 **ResW** 檔案有 MainPage.FlowDirection 的資源，您可以針對要當地語系化的每個語言設定這個資源。


## <a name="mirroring-images"></a>鏡像影像

如果應用程式含有必須針對 RTL 進行鏡像的影像 (也就是相同的影像可以翻轉)，您可以套用 [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 屬性：

```XML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```


如果應用程式需要不同的影像才能正確地翻轉影像，您可以使用資源管理系統搭配 [layoutdir 辨識符號](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。 系統會在[應用程式語言](manage-language-and-region.md)設定為 RTL 語言時，選擇名為 file.layoutdir-rtl.png 的影像。 在已翻轉影像的某些部分，但其他部分尚未翻轉時，可能需要這個處理方式。

## <a name="fonts"></a>字型

您可以使用 [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) 字型對應 API 以程式設計方式存取特定語言的建議字型系列、大小、粗細及樣式。 **LanguageFont** 物件會針對各種內容類別 (包括 UI 標頭、通知、內文，以及使用者可以編輯的文件主體字型)，提供正確字型資訊的存取權。

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>處理從右至左 (RTL) 語言的最佳做法

當您的 app 要針對從右至左 (RTL) 語言進行當地語系化時，可以使用 API 來設定 RootFrame 的預設文字方向。 這會讓包含在 RootFrame 中的所有控制項適當地回應預設文字方向。  支援多種語言時，請使用最慣用語言的 LayoutDirection 來設定 FlowDirection 屬性。 Windows 中包含的大部分控制項均已使用 FlowDirection。 如果您要實作自訂控制項，則這類控制項應使用 FlowDirection，以針對 RTL 及 LTR 語言配置進行適當地變更。

C#
```csharp    
// For bidirectional languages, determine flow direction for RootFrame and all derived UI.

    string resourceFlowDirection = ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
    if (resourceFlowDirection == "LTR")
    {
       RootFrame.FlowDirection = FlowDirection.LeftToRight;
    }
    else
    {
       RootFrame.FlowDirection = FlowDirection.RightToLeft;
    }
```

C++：
```
    // Get preferred app language
    m_language = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);
     
    // Set flow direction accordingly
    m_flowDirection = ResourceManager::Current->DefaultContext->QualifierValues->Lookup("LayoutDirection") != "LTR" ? 
       FlowDirection::RightToLeft : FlowDirection::LeftToRight;
```


### <a name="rtl-faq"></a>RTL 常見問題集 

<dl>
  <dt> <p><b>問：</b><b>FlowDirection</b> 是否會根據目前所選取的語言自動設定？ 例如，如果我選取英文，它將會從左至右顯示，而如果我選取阿拉伯文，它是否會從右至左顯示？</p></dt>

  <dd><p><b>答：</b> <b>FlowDirection</b> 不會將語言納入考量。 您要針對目前所顯示的語言適當地設定 <b>FlowDirection</b>。 請參閱上述範例程式碼。</p></dd> 

  <dt> <p><b>問︰</b>我不太熟悉當地語系化。 資源是否已經包含文字方向？ 是否可能從目前的語言判斷文字方向？</p></dt>

  <dd> <p><b>答︰</b>如果您使用目前的最佳做法，則資源並未直接包含文字方向。 您必須判斷目前語言的文字方向。 以下有兩種做法： </p>
   <p>最好的方法是使用最慣用語言的 LayoutDirection 來設定 RootFrame 的 FlowDirection 屬性。 RootFrame 中的所有控制項都會從 RootFrame 繼承 FlowDirection。</p>
   <p>另一種方法是在您要進行當地語系化的 RTL 語言資源檔 (resw 檔案) 中設定 FlowDirection。 例如，您可能會有阿拉伯文的 resw 檔案及希伯來文的 resw 檔案。 您可以在這些檔案中使用 x:UID 來設定 FlowDirection。 但這個方法會比程式設計方法更容易發生錯誤。</p></dd>
</dl>


## <a name="related-topics"></a>相關主題
[FlowDirection](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.flowdirection.aspx)
