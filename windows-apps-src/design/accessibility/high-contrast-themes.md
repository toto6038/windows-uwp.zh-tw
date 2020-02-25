---
description: 描述使用高對比佈景主題時，確保通用 Windows 平台 (UWP) App 可以正常運作所需的步驟。
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: 高對比佈景主題
template: detail.hbs
ms.date: 09/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 634f85ec64597f14210cf83fd67189f2f54bad4d
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521249"
---
# <a name="high-contrast-themes"></a>高對比佈景主題  

Windows 支援使用者可選擇啟用的作業系統和 App 高對比佈景主題。 高對比佈景主題使用色彩種類少的對比色，讓介面更容易分辨。

![淺色佈景主題和「黑底白字」佈景主題中的「小算盤」。](images/high-contrast-calculators.png)

*[淺色主題] 和 [高對比黑色主題] 中所顯示的計算機。*

您可以使用 [設定] &gt; [輕鬆存取] &gt; [高對比]，切換成高對比佈景主題。

> [!NOTE]
> 請留意，高對比佈景主題與淺色和深色佈景主題不同，後兩者使用較多種色彩，且不一定是高對比。 如需淺色和深色佈景主題的詳細資訊，請參閱關於[色彩](../style/color.md)的文章。

雖然常用控制項都免費支援完整的高對比，但自訂 UI 時還是需要注意。 造成高對比錯誤的最常見原因，是以內嵌和硬式編碼方式在控制項上設定色彩。

```xaml
<!-- Don't do this! -->
<Grid Background="#E6E6E6">

<!-- Instead, create BrandedPageBackgroundBrush and do this. -->
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

在第一個範例中以內嵌方式設定色彩 `#E6E6E6` 時，該格線在所有佈景主題中都會保持該背景色彩。 如果使用者切換成「黑底白字」佈景主題，他們會預期 App 具有黑色背景。 由於 `#E6E6E6` 很接近白色，某些使用者可能會無法與您的 App 互動。

在第二個範例中，[ **{ThemeResource} 標記延伸**](../../xaml-platform/themeresource-markup-extension.md)是用來參考 [**ThemeDictionaries**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) 集合 ([**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) 元素的專用屬性) 中的某個色彩。 **ThemeDictionaries** 讓 XAML 能根據使用者目前的佈景主題，自動為您切換色彩。

## <a name="theme-dictionaries"></a>佈景主題字典

當您需要變更系統預設色彩時，請針對您的 App 建立 ThemeDictionaries 集合。

1. 由建立適當的配置開始 (如果尚未存在)。 在 App.xaml 中，建立 **ThemeDictionaries** 集合，其中至少包含 **Default** 與 **HighContrast**。
2. 在 **Default** 中，建立您需要的 [Brush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush) 類型 (通常是 **SolidColorBrush**)。 針對它的用途來指定 *x:Key* 名稱。
3. 指派您想要的**色彩**。
4. 將該 **Brush** 標記複製到 **HighContrast** 中。

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

最後步驟是決定要在高對比中使用的色彩，下一個章節中會說明。

> [!NOTE]
> **HighContrast** 不是唯一可用的索引鍵名稱。 另外還有 **HighContrastBlack**、**HighContrastWhite** 以及 **HighContrastCustom**。 在大部分情況下，您只需要 **HighContrast**。

## <a name="high-contrast-colors"></a>高對比色彩

在 *[設定] > [輕鬆存取] > [高對比]* 頁面中，有 4 個預設的高對比佈景主題。 


![高對比設定](images/high-contrast-settings.png)  

*在使用者選取選項之後，頁面會顯示預覽。*  

![高對比資源](images/high-contrast-resources.png)  

*您可以按一下預覽中的每個色樣來變更其值。每個色板也會直接對應至 XAML 色彩資源。*  

每個 **SystemColor*Color** 資源都是變數，當使用者切換高對比佈景主題時會自動更新色彩。 以下是在何處及何時使用各項資源的指導方針。

資源 | 使用方式 |
|--------|-------|
**SystemColorWindowTextColor** | 內文文字、標題、清單；任何無法進行互動的文字 |
| **SystemColorHotlightColor** | 超連結 |
| **SystemColorGrayTextColor** | 停用的 UI |
| **SystemColorHighlightTextColor** | 狀態是處理中、已選取，或目前正在互動之文字或 UI 的前景色彩 |
| **SystemColorHighlightColor** | 狀態是處理中、已選取，或目前正在互動之文字或 UI 的背景色彩 |
| **SystemColorButtonTextColor** | 按鈕的前景色彩；可以進行互動的任何 UI |
| **SystemColorButtonFaceColor** | 按鈕的背景色彩；可以進行互動的任何 UI |
| **SystemColorWindowColor** | 頁面、窗格、快顯視窗，以及列的背景 |

查看現有的 App、[開始]，或常用控制項通常很有幫助，可以了解其他人如何解決類似的高對比設計問題。

**對**

* 盡可能遵守前景/背景組合。
* 在您的 App 執行時，測試全部 4 種高對比佈景主題。 使用者切換佈景主題時，應不需要重新啟動您的 App。
* 保持一致。

**不要**

* 在 **HighContrast** 佈景主題中以硬式編碼設定色彩；請使用 **SystemColor*Color** 資源。
* 依照美學來選擇色彩資源。 請記住，色彩會隨佈景主題變更！
* 請勿將 **SystemColorGrayTextColor** 用於本文文字，它應用於次要文字或當作提示。


若要繼續先前的範例，您需要挑選 **BrandedPageBackgroundBrush** 的資源。 因為該名稱指出它會用於背景，所以 **SystemColorWindowColor** 是不錯的選擇。

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

稍後您可以在您的 App 中設定背景。

```xaml
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

請注意 **\{ThemeResource\}** 如何使用兩次，一次是參考**SystemColorWindowColor** ，另一個是參考**BrandedPageBackgroundBrush**。 兩次都是為了讓您的 App 在執行階段能正確設定佈景主題。 現在是測試您 App 內功能的好時機。 當您切換成高對比佈景主題時，格線的背景將會自動更新。 在不同的高對比佈景主題之間切換時，它也會更新。

## <a name="when-to-use-borders"></a>使用框線的時機

在高對比佈景主題中，頁面、窗格、快顯視窗和列，都應使用 **SystemColorWindowColor** 做為其背景。 如需保留您 UI 中的重要框線，請新增高對比專用的框線。

![與頁面的其餘部分劃分的瀏覽窗格](images/high-contrast-actions-content.png)  

*流覽窗格和頁面都共用相同的背景色彩（高對比）。若要將其劃分為高對比的框線，這是不可或缺的。*


## <a name="list-items"></a>清單項目

在高對比佈景主題中，當游標暫留、按下或選取 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 中的項目時，其背景會設為 **SystemColorHighlightColor**。 複雜的清單項目通常會有一種錯誤，就是當游標暫留、按下或選取清單項目時，沒有反轉其內容的色彩。 這會使該項目難以閱讀。

![淺色佈景主題和「黑底白字」佈景主題中的簡易清單](images/high-contrast-list1.png)

*淺色主題的簡單列表（左）和高對比黑色主題（right）。第二個專案已選取;請注意其文字色彩在高對比中的反轉方式。*


### <a name="list-items-with-colored-text"></a>包含文字色彩的清單項目

問題的其中一個癥結是在 ListView 的 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 中設定 TextBlock.Foreground。 這通常是用來建立視覺階層。 Foreground 屬性是在 [ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem) 上設定，而在游標暫留、按下或選取項目時，DataTemplate 中的 TextBlocks 會繼承正確的 Foreground 色彩。 不過，設定 Foreground 會中斷繼承。

![淺色佈景主題和「黑底白字」佈景主題中的複雜清單](images/high-contrast-list2.png)

*淺色主題的複雜清單（左）和高對比黑色主題（right）。在 [高對比] 中，選取專案的第二行無法反轉。*  

您可以視情況透過 **ThemeDictionaries** 集合中的 Style 設定 Foreground，來解決此問題。 因為 **HighContrast** 中的 **Foreground** 不是由 **SecondaryBodyTextBlockStyle** 來設定，所以它的顏色將會正確反轉。

```xaml
<!-- In App.xaml... -->
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <!-- The Foreground Setter is omitted in HighContrast -->
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}" />
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>

<!-- Usage in your DataTemplate... -->
<DataTemplate>
    <StackPanel>
        <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Double line list item" />

        <!-- Note how ThemeResource is used to reference the Style -->
        <TextBlock Style="{ThemeResource SecondaryBodyTextBlockStyle}" Text="Second line of text" />
    </StackPanel>
</DataTemplate>
```


## <a name="detecting-high-contrast"></a>偵測高對比

您可使用 [**AccessibilitySettings**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings) 類別的成員，以程式設計的方式檢查目前的佈景主題是否為高對比。

> [!NOTE]
> 請確定您是從 App 已經初始化且已經顯示內容的範圍內呼叫 **AccessibilitySettings** 建構函式。

## <a name="related-topics"></a>相關主題  
* [協助工具](accessibility.md)
* [UI 對比和設定範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20high%20contrast%20style%20sample%20(Windows%208))
* [XAML 協助工具範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [XAML 高對比範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20high%20contrast%20style%20sample%20(Windows%208))
* [**AccessibilitySettings**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings)
