---
author: QuinnRadich
description: 了解如何在 UWP app 中使用輔色及佈景主題。
title: UWP app 中的色彩
ms.author: quradic
ms.date: 4/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
design-contact: karenmui
ms.localizationpriority: medium
ms.openlocfilehash: 19f4d9cde6ee2bc9615f044f18bc5e8828ca1985
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2018
ms.locfileid: "3821322"
---
# <a name="color"></a>色彩

![主角圖像](images/header-color.svg)

色彩可提供直覺的溝通資訊方式給您應用程式中的使用者：色彩可用來指出互動性、提供意見反應給使用者動作，以及讓您的介面產生視覺延續性。 

在 UWP app 中，色彩主要由輔色及佈景主題來決定。 在本文中，我們會討論如何在應用程式中使用色彩，以及如何使用輔色及佈景主題資源，讓您的 UWP app 可用於任何佈景主題背景。 

## <a name="color-principles"></a>色彩原則

:::row:::
    :::column:::
        **有意義地使用色彩。**
謹慎使用色彩來醒目提示重要元素，可以協助建立流暢、直覺的使用者介面。
    :::column-end:::
    :::column:::
        **使用色彩來表示互動性。**
最好選擇一種色彩來表示您應用程式中的可互動元素。 例如，許多網頁使用藍色文字來表示超連結。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **色彩是個人化。**
在 Windows 中，使用者可以選擇在他們的體驗中要反映的輔色和淺色或深色佈景主題。 您可以選擇如何將使用者的輔色及佈景主題整合到您的應用程式中，以提供個人化的體驗。
    :::column-end:::
    :::column:::
        **色彩跟文化有關。**
請考慮您使用的色彩會如何被來自不同文化背景的人解讀。 例如，在某些文化中，藍色代表美德和保護，但在其他文化中代表服喪。
    :::column-end:::
:::row-end:::

## <a name="themes"></a>佈景主題

UWP app 可以使用淺色或深色應用程式佈景主題。 佈景主題會影響應用程式的的背景、文字、圖示和[通用控制項](../controls-and-patterns/index.md)。

### <a name="light-theme"></a>淺色佈景主題

![淺色佈景主題](images/color/light-theme.svg)

### <a name="dark-theme"></a>深色佈景主題

![深色佈景主題](images/color/dark-theme.svg)

根據預設，您的 UWP app 佈景主題是 Windows 設定中使用者的佈景主題喜好設定，或裝置的預設佈景主題 (也就是 XBox 上的深色)。 不過，您可以設定適用於 UWP app 的佈景主題。 

### <a name="changing-the-theme"></a>變更佈景主題

您可以透過變更 `App.xaml` 檔案中的 **RequestedTheme** 屬性來變更佈景主題。

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

移除 **RequestedTheme** 屬性表示您的應用程式將會使用使用者的系統設定。

使用者也可以選取高對比佈景主題以使用色彩種類少的對比色，讓介面更容易分辨。 在此情況下，系統將會覆寫您的 RequestedTheme。

### <a name="testing-themes"></a>測試佈景主題

如果您未要求適用於您應用程式的佈景主題，請務必在深色和淺色佈景主題中測試您的應用程式，以確保您的應用程式在任何情況下都清晰可見。

**注意**：在 Visual Studio 中，預設 RequestedTheme 是淺色，因此您必須變更 RequestedTheme 以測試兩者。

## <a name="theme-brushes"></a>佈景主題筆刷

通用控制項會自動使用[佈景主題筆刷](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes)來調整深色和淺色佈景主題的對比。

例如，以下說明 [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 如何使用佈景主題筆刷：

![佈景主題筆刷控制項範例](images/color/theme-brushes.svg)

佈景主題筆刷適用於下列目的：

- **Base** 適用於文字。
- **Alt** 是 Base 的反轉。
- **Chrome** 適用於最上層元素，例如瀏覽窗格或命令列。
- **List** 適用於清單控制項。

**Low**/**Medium**/**High** 表示色彩濃度。

### <a name="using-theme-brushes"></a>使用佈景主題筆刷

:::row:::
    :::column:::
        當建立自訂控制項的範本，請使用佈景主題筆刷，而非硬式色彩值。 如此一來，您的應用程式可以輕鬆地適應任何佈景主題。

        For example, these [item templates for ListView](../controls-and-patterns/item-templates-listview.md) demonstrate how to use theme brushes in a custom template.
    :::column-end:::
    :::column:::
         ![double line list item with icon example](images/color/list-view.svg)
    :::column-end:::
:::row-end:::

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

如需在應用程式中使用佈景主題筆刷的詳細資訊，請參閱[佈景主題資源](../controls-and-patterns/xaml-theme-resources.md)。

## <a name="accent-color"></a>輔色

通用控制項可使用輔色來傳遞狀態資訊。 根據預設，輔色是使用者在他們的設定中選取的 `SystemAccentColor`。 不過，您也可以自訂應用程式的輔色，以反映品牌。

![視窗控制項](images/color/windows-controls.svg)

:::row:::
    :::column:::
        ![使用者選取的輔色標頭](images/color/user-accent.svg)![使用者選取的輔色](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![自訂輔色標頭](images/color/custom-accent.svg)![自訂品牌輔色](images/color/brand-color.svg)
    :::column-end:::
:::row-end:::

### <a name="overriding-the-accent-color"></a>覆寫輔色

若要變更您應用程式的輔色，請更換 `app.xaml` 中的下列程式碼。

```xaml
<Application.Resources>
    <ResourceDictionary>
        <Color x:Key="SystemAccentColor">#107C10</Color>
    </ResourceDictionary>
</Application.Resources>
```

### <a name="choosing-an-accent-color"></a>選擇輔色

如果您為您的應用程式選取自訂輔色，請確保使用輔色的文字和背景具有足夠對比，以獲得最佳可讀性。 若要測試對比，您可以使用 Windows 設定中的色彩選擇器工具，或者也可以使用這些[線上對比工具](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)。

![Windows 設定自訂輔色選擇器](images/color/color-picker.svg)

## <a name="accent-color-palette"></a>輔色調色盤

Windows 殼層中的輔色演算法會產生輔色的淺色和深色色調。

![輔色調色盤](images/color/accent-color-palette.svg)

這些色調可用[佈景主題資源 ](../controls-and-patterns/xaml-theme-resources.md)存取：

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true -->
您也可以使用 [**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) 方法和 [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType) 列舉，以程式設計方式存取輔色調色盤。

您可以使用輔色調色盤，在您的應用程式中進行色彩佈景主題設定。 以下是如何在按鈕上使用輔色調色盤的範例。

![套用至按鈕的輔色調色盤](images/color/color-theme-button.svg)

```xaml
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ButtonBackground" Color="{ThemeResource SystemAccentColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver" Color="{ThemeResource SystemAccentColorLight1}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed" Color="{ThemeResource SystemAccentColorDark1}"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>

<Button Content="Button"></Button>
```

在彩色背景上使用彩色文字時，請確定文字與背景之間有足夠對比。 根據預設，超連結或超文字會使用輔色。 如果您套用輔色的變化至背景，您應該使用原始輔色的變化以在彩色背景的彩色文字上獲得最佳對比。

下圖顯示輔色的各種淺色/深色色調範例，以及如何在彩色表面上套用彩色類型。

![色彩上的色彩](images/color/color-on-color.svg)

如需控制項樣式設定的詳細資訊，請參閱 [XAML 樣式](../controls-and-patterns/xaml-styles.md)。

## <a name="color-api"></a>色彩 API

有數個可用來增加應用程式色彩的 API。 首先是 [**Colors**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors) 類別，可實作預先定義色彩的大型清單。 這些都可使用 XAML 屬性自動存取。 在以下範例中，我們建立按鈕並設定背景和前景色彩屬性為 **Colors** 類別成員。

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

您可以使用 XAML 中的 [**Color**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.color) 結構，從 RGB 或十六進位值建立自己的色彩。

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

您也可以使用 **FromArgb** 方法，在程式碼中建立相同的色彩。

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

字母「Argb」表示 Alpha (透明度)、紅色、綠色和藍色，色彩的四個元件。 每個引數的範圍為 0 到 255。 您可以選擇省略第一個值，如此會提供 255 的預設不透明度，亦即 100% 不透明。

> [!Note]
> 如果您使用 C++，則必須使用 [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper) 類別建立色彩。

**Color** 的最常見用途是做為 [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush) 的引數，可使用單一純色來繪製 UI 元素。 這些筆刷一般定義在 [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary) 中，因此可重複使用於多個元素。

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

如需如何使用筆刷的詳細資訊，請參閱 [XAML 筆刷](brushes.md)。

## <a name="usability"></a>可用性

:::row:::
    :::column:::
        ![對比圖例](images/color/illo-contrast.svg)
    :::column-end:::
    ::: 欄範圍 ="2":::**對比**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![對比圖例](images/color/illo-lighting.svg)
    :::column-end:::
    ::: 欄範圍 ="2":::**光源**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![對比圖例](images/color/illo-colorblindness.svg)
    :::column-end:::
    ::: 欄範圍 ="2":::**色盲**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>相關文章

- [XAML 樣式](../controls-and-patterns/xaml-styles.md)
- [XAML 佈景主題資源](../controls-and-patterns/xaml-theme-resources.md)