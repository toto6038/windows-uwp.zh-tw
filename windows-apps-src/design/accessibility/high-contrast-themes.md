---
description: 描述使用高對比佈景主題時，確保通用 Windows 平台 (UWP) App 可以正常運作所需的步驟。
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: 高對比佈景主題
template: detail.hbs
ms.date: 09/28/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b58eb4b6e3f3f02bb1f72fcba9da3710f08a72da
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649003"
---
# <a name="high-contrast-themes"></a>高對比佈景主題  

Windows 支援使用者可選擇啟用的作業系統和 App 高對比佈景主題。 高對比佈景主題使用色彩種類少的對比色，讓介面更容易分辨。

![淺色佈景主題和「黑底白字」佈景主題中的「小算盤」。](images/high-contrast-calculators.png)

*淺色佈景主題和黑色高對比佈景主題中所示的計算機。*

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

在第二個範例中，[**{ThemeResource} 標記延伸**](../../xaml-platform/themeresource-markup-extension.md)是用來參考 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.themedictionaries.aspx) 集合 ([**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 元素的專用屬性) 中的某個色彩。 **ThemeDictionaries** 讓 XAML 能根據使用者目前的佈景主題，自動為您切換色彩。

## <a name="theme-dictionaries"></a>佈景主題字典

當您需要變更系統預設色彩時，請針對您的 App 建立 ThemeDictionaries 集合。

1. 由建立適當的配置開始 (如果尚未存在)。 在 App.xaml 中，建立 **ThemeDictionaries** 集合，其中至少包含 **Default** 與 **HighContrast**。
2. 在 **Default** 中，建立您需要的 [Brush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx) 類型 (通常是 **SolidColorBrush**)。 針對它的用途來指定 *x:Key* 名稱。
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

在 [設定] &gt; [輕鬆存取] &gt; [高對比] 頁面中，有 4 個預設的高對比佈景主題。 


![高對比設定](images/high-contrast-settings.png)  

*在使用者選取一個選項之後，頁面會顯示預覽。*  

![高對比資源](images/high-contrast-resources.png)  

*若要變更其值，可以按一下每個預覽上的色樣。每個樣本也可以直接對應到 XAML 色彩資源。*  

每個 **SystemColor*Color** 資源都是變數，當使用者切換高對比佈景主題時會自動更新色彩。 以下是在何處及何時使用各項資源的指導方針。

資源 | 用途 |
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

**執行**

* 盡可能遵守前景/背景組合。
* 在您的 App 執行時，測試全部 4 種高對比佈景主題。 使用者切換佈景主題時，應不需要重新啟動您的 App。
* 保持一致。

**不**

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

請注意如何 **\{ThemeResource\}** 兩次，用來參考一次 **SystemColorWindowColor** 並再次參考 **BrandedPageBackgroundBrush**. 兩次都是為了讓您的 App 在執行階段能正確設定佈景主題。 現在是測試您 App 內功能的好時機。 當您切換成高對比佈景主題時，格線的背景將會自動更新。 在不同的高對比佈景主題之間切換時，它也會更新。

## <a name="when-to-use-borders"></a>使用框線的時機

在高對比佈景主題中，頁面、窗格、快顯視窗和列，都應使用 **SystemColorWindowColor** 做為其背景。 如需保留您 UI 中的重要框線，請新增高對比專用的框線。

![與頁面的其餘部分劃分的瀏覽窗格](images/high-contrast-actions-content.png)  

*瀏覽窗格和頁面都共用相同的背景色彩在高對比。將它們的高對比專用框線是不可或缺的。*


## <a name="list-items"></a>清單項目

在高對比佈景主題中，當游標暫留、按下或選取 [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 中的項目時，其背景會設為 **SystemColorHighlightColor**。 複雜的清單項目通常會有一種錯誤，就是當游標暫留、按下或選取清單項目時，沒有反轉其內容的色彩。 這會使該項目難以閱讀。

![淺色佈景主題和「黑底白字」佈景主題中的簡易清單](images/high-contrast-list1.png)

*淺色佈景主題 （左） 和 （右） 的黑色高對比佈景主題中的簡單清單。選取第二個項目;請注意如何在高對比反轉其文字色彩。*


### <a name="list-items-with-colored-text"></a>包含文字色彩的清單項目

問題的其中一個癥結是在 ListView 的 [DataTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 中設定 TextBlock.Foreground。 這通常是用來建立視覺階層。 Foreground 屬性是在 [ListViewItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) 上設定，而在游標暫留、按下或選取項目時，DataTemplate 中的 TextBlocks 會繼承正確的 Foreground 色彩。 不過，設定 Foreground 會中斷繼承。

![淺色佈景主題和「黑底白字」佈景主題中的複雜清單](images/high-contrast-list2.png)

*淺色佈景主題 （左） 和黑色高對比佈景主題 （右邊） 中的複雜清單。在高對比，第二行的選取項目無法反轉。*  

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

您可使用 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 類別的成員，以程式設計的方式檢查目前的佈景主題是否為高對比。

> [!NOTE]
> 請確定您是從 App 已經初始化且已經顯示內容的範圍內呼叫 **AccessibilitySettings** 建構函式。

## <a name="related-topics"></a>相關主題  
* [協助工具](accessibility.md)
* [UI 對比和設定範例](https://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML 的協助工具範例](https://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML 高對比範例](https://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)
