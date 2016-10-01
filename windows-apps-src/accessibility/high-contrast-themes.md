---
author: Xansky
description: "描述使用高對比佈景主題時，確保通用 Windows 平台 (UWP) App 可以正常運作所需的步驟。"
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: "高對比佈景主題"
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: f3da82cab8813653a6ee999976983937649b42b2
ms.openlocfilehash: 30785998d11f09ef94f33789e3e74b0933d9c83e

---

# 高對比佈景主題  

Windows 支援使用者可選擇啟用的作業系統和 App 高對比佈景主題。 高對比佈景主題使用色彩種類少的對比色，讓介面更容易分辨。

**圖 1. 淺色佈景主題和「黑底白字」佈景主題中的「小算盤」。**

![淺色佈景主題和「黑底白字」佈景主題中的「小算盤」。](images/high-contrast-calculators.png)


您可以使用 [設定] &gt; [輕鬆存取] &gt; [高對比]**，切換成高對比佈景主題。

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

在第二個範例中，[**{ThemeResource} 標記延伸**](../xaml-platform/themeresource-markup-extension.md)是用來參考 [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.themedictionaries.aspx) 集合 ([**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 元素的專用屬性) 中的某個色彩。 ThemeDictionaries 讓 XAML 能根據使用者目前的佈景主題，自動為您切換色彩。

## 佈景主題字典

當您需要變更系統預設色彩時，請針對您的 App 建立 ThemeDictionaries 集合。

1. 由建立適當的配置開始 (如果尚未存在)。 在 App.xaml 中，建立 ThemeDictionaries 集合，其中至少包含 **Default** 與 **HighContrast**。
2. 在 Default 中，建立您需要的 [Brush](http://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx) 類型 (通常是 SolidColorBrush)。 針對它的用途來指定 x:Key 名稱。
3. 指派您想要的色彩。
4. 將該 Brush 標記複製到 HighContrast 中。

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
> HighContrast 不是唯一可用的索引鍵名稱。 另外還有 HighContrastBlack、HighContrastWhite 以及 HighContrastCustom。 在大部分情況下，您只需要 HighContrast。

## 高對比色彩

在 [設定] &gt; [輕鬆存取] &gt; [高對比]** 頁面中，有 4 個預設的高對比佈景主題。 

**圖 2. 在使用者選取一個選項之後，頁面就會顯示預覽。**

![高對比設定](images/high-contrast-settings.png)

**圖 3. 預覽上的每一個色樣都可以按一下來變更其值。 每一個色樣也都直接對應到 XAML 資源。**

![高對比資源](images/high-contrast-resources.png)

每個 `SystemColor*Color` 資源都是變數，當使用者切換高對比佈景主題時會自動更新色彩。 以下是在何處及何時使用各項資源的指導方針。

資源 | 使用方式
-------- | -----
SystemColorWindowTextColor | 內文文字、標題、清單；任何無法進行互動的文字
SystemColorHotlightColor | 超連結
SystemColorGrayTextColor | 停用的 UI
SystemColorHighlightTextColor | 狀態是處理中、已選取，或目前正在互動之文字或 UI 的前景色彩
SystemColorHighlightColor | 狀態是處理中、已選取，或目前正在互動之文字或 UI 的背景色彩
SystemColorButtonTextColor | 按鈕的前景色彩；可以進行互動的任何 UI
SystemColorButtonFaceColor | 按鈕的背景色彩；可以進行互動的任何 UI
SystemColorWindowColor | 頁面、窗格、快顯視窗，以及列的背景
<br/>
查看現有的 App、[開始]，或常用控制項通常很有幫助，可以了解其他人如何解決類似的高對比設計問題。

**可行事項**

* 盡可能遵守前景/背景組合。
* 在您的 App 執行時，測試全部 4 種高對比佈景主題。 使用者切換佈景主題時，應不需要重新啟動您的 App。
* 保持一致。

**禁止事項**

* 在 HighContrast 佈景主題中以硬式編碼設定色彩；請使用 `SystemColor*Color` 資源。
* 依照美學來選擇色彩資源。 請記住，色彩會隨佈景主題變更！
* 請勿將 `SystemColorGrayTextColor` 用於本文文字，它應用於次要文字或當作提示。


若要繼續先前的範例，您需要挑選 `BrandedPageBackgroundBrush` 的資源。 因為該名稱指出它會用於背景，所以 `SystemColorWindowColor` 是不錯的選擇。

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

請注意使用兩次 `{ThemeResource}` 的方式：一次是參考 `SystemColorWindowColor`，另一次是參考 `BrandedPageBackgroundBrush`。 兩次都是為了讓您的 App 在執行階段能正確設定佈景主題。 現在是測試您 App 內功能的好時機。 當您切換成高對比佈景主題時，格線的背景將會自動更新。 在不同的高對比佈景主題之間切換時，它也會更新。

## 使用框線的時機

在高對比佈景主題中，頁面、窗格、快顯視窗和列，都應使用 `SystemColorWindowColor` 做為其背景。 如需保留您 UI 中的重要框線，請新增高對比專用的框線。

**圖 4. 瀏覽窗格與頁面在高對比佈景主題中共用相同的背景色彩。 此時需要有高對比專用的框線來劃分它們。**

![與頁面的其餘部分劃分的瀏覽窗格](images/high-contrast-actions-content.png)

## 清單項目

在高對比佈景主題中，當游標暫留、按下或選取 [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 中的項目時，其背景會設為 `SystemColorHighlightColor`。 複雜的清單項目通常會有一種錯誤，就是當游標暫留、按下或選取清單項目時，沒有反轉其內容的色彩。 這會使該項目難以閱讀。

**圖 5. 淺色佈景主題 (左) 和「黑底白字」佈景主題 (右) 中的簡易清單。 已選取第二個項目，請注意在高對比模式中，其文字色彩反轉的方式。**

![淺色佈景主題和「黑底白字」佈景主題中的簡易清單](images/high-contrast-list1.png)



### 包含文字色彩的清單項目

問題的其中一個癥結是在 ListView 的 [DataTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 中設定 TextBlock.Foreground。 這通常是用來建立視覺階層。 Foreground 屬性是在 [ListViewItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) 上設定，而在游標暫留、按下或選取項目時，DataTemplate 中的 TextBlocks 會繼承正確的 Foreground 色彩。 不過，設定 Foreground 會中斷繼承。

**圖 6. 淺色佈景主題 (左) 和「黑底白字」佈景主題 (右) 中的複雜清單。 請注意高對比佈景主題中，已選取項目的第二行文字沒有反轉色彩。**

![淺色佈景主題和「黑底白字」佈景主題中的複雜清單](images/high-contrast-list2.png)

您可以視情況透過 ThemeDictionaries 集合中的 Style 設定 Foreground，來解決此問題。 因為 HighContrast 中的 Foreground 不是由 SecondaryBodyTextBlockStyle 來設定，所以它的顏色將會正確反轉。

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

### 包含按鈕和連結的清單項目

有時候清單項目中會包含更複雜的控制項，例如 [HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.hyperlinkbutton.aspx) 或 [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)。 這些控制項有各自的游標暫留、按下和選取 (不一定有) 狀態，而它們不會在清單項目之上運作。 「黑底白字」佈景主題中的超連結是黃色的，會使它們在滑鼠指標暫留、按下或選取項目清單時，變得難以閱讀。

**圖 7. 請留意高對比佈景主題中的超連結為何難以閱讀。**

![淺色佈景主題和「黑底白字」佈景主題中包含超連結的複雜清單](images/high-contrast-list3.png)

一種解決方案是在高對比佈景主題中，將 DataTemplate 的背景設為 `SystemColorWindowColor`。 這會在高對比佈景主題中產生框線的效果。

```xaml
<!-- In App.xaml... -->
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <SolidColorBrush x:Key="HighContrastOnlyBackgroundBrush" Color="Transparent" />
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <SolidColorBrush x:Key="HighContrastOnlyBackgroundBrush" Color="{ThemeResource SystemColorWindowColor}" />
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>

<!-- Usage in your ListView... -->
<ListView>
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <!-- Causes the DataTemplate to fill the entire width and height
            of the list item -->
            <Setter Property="HorizontalContentAlignment" Value="Stretch" />
            <Setter Property="VerticalContentAlignment" Value="Stretch" />

            <!-- Padding is handled in the DataTemplate -->
            <Setter Property="Padding" Value="0" />
        </Style>
    </ListView.ItemContainerStyle>
    <ListView.ItemTemplate>
        <DataTemplate>
            <!-- Margin of 2px allows some of the ListViewItem's background
            to shine through. An additional left padding of 10px puts the
            content a total of 12px from the left edge -->
            <StackPanel
                Margin="2,2,2,2"
                Padding="10,0,0,0"
                Background="{ThemeResource HighContrastOnlyBackgroundBrush}">

                <!-- Foreground is explicitly set so that it doesn't
                disappear on hovered, pressed, or selected -->
                <TextBlock
                    Foreground="{ThemeResource SystemControlForegroundBaseHighBrush}"
                    Text="Double line list item" />

                <HyperlinkButton Content="Hyperlink" />
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```
**圖 8. 當您的清單項目中有較複雜的控制項時，框線效果是不錯的做法。**

![淺色佈景主題和「黑底白字」佈景主題中包含超連結的清單 (已修正)](images/high-contrast-list4.png)



## 偵測高對比

您可使用 [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) 類別的成員，以程式設計的方式檢查目前的佈景主題是否為高對比。

> [!NOTE]
> 請確定您是從 App 已經初始化且已經顯示內容的範圍內呼叫 **AccessibilitySettings** 建構函式。

## 相關主題  
* [協助工具](accessibility.md)
* [UI 對比和設定範例](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML 高對比範例](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)



<!--HONumber=Aug16_HO3-->


