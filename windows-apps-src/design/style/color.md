---
description: 了解如何在 UWP app 中使用輔色及佈景主題。
title: UWP app 中的色彩
ms.date: 04/7/2018
ms.topic: article
keywords: Windows 10, UWP
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 49d891888e26b6ce4c9f94e92605eaf7d619b6f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654253"
---
# <a name="color"></a>色彩

![主角圖像](images/header-color.svg)

色彩可提供直覺的溝通資訊方式給您應用程式中的使用者：色彩可用來指出互動性、提供意見反應給使用者動作，以及讓您的介面產生視覺延續性。 

在 UWP app 中，色彩主要由輔色及佈景主題來決定。 在本文中，我們會討論如何在應用程式中使用色彩，以及如何使用輔色及佈景主題資源，讓您的 UWP app 可用於任何佈景主題背景。 

## <a name="color-principles"></a>色彩原則

:::row:::
    :::column:::
        **Use color meaningfully.**
        When color is used sparingly to highlight important elements, it can help create a user interface that is fluid and intuitive.
    :::column-end:::
    :::column:::
        **Use color to indicate interactivity.**
        It's a good idea to choose one color to indicate elements of your application that are interactive. For example, many web pages use blue text to denote a hyperlink.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Color is personal.**
        In Windows, users can choose an accent color and a light or dark theme, which are reflected throughout their experience. You can choose how to incorporate the user's accent color and theme into your application, personalizing their experience.
    :::column-end:::
    :::column:::
        **Color is cultural.**
        Consider how the colors you use will be interpreted by people from different cultures. For example, in some cultures the color blue is associated with virtue and protection, while in others it represents mourning.
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

**注意**：在 Visual Studio 中，預設值 RequestedTheme 為光線，因此您必須變更以同時測試 RequestedTheme。

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
        When creating templates for custom controls, use theme brushes rather than hard code color values. This way, your app can easily adapt to any theme.

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
        ![user-selected accent header](images/color/user-accent.svg)
        ![user-selected accent color](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![custom accent header](images/color/custom-accent.svg)
        ![custom brand accent color](images/color/brand-color.svg)
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

<!-- check this is true --> 您也可以存取的強調色彩調色盤，以程式設計方式用[ **UISettings.GetColorValue** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_)方法並[ **UIColorType** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType)列舉。

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

![色彩上的色彩](images/color/color-on-color.png)

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

## <a name="scoping-system-colors"></a>範圍的系統色彩

除了在應用程式中定義您自己的色彩，您可以也我們 systematized 的色彩，想要的區域以整個應用程式設定的範圍使用**ColorSchemeResources**標記。 此 API 可讓您不僅要以顏色標示和佈景主題的大型群組的一次是藉由設定一些屬性，但也提供您許多其他系統有益於您的控制項通常不會收到與手動定義您自己自訂的色彩：

- 使用設定的任何色彩**ColorSchemeResources**不會影響高對比
  * 這表示您的應用程式將可存取更多的人，而不需要任何額外的設計或開發成本
- 可以輕鬆地設定色彩為淺、 暗色或普遍在這兩個佈景主題藉由在 API 上設定一個屬性
- 在 色彩集**ColorSchemeResources**會串聯到所有類似的控制項，也會使用該系統色彩
  * 這確保您會有一致的色彩劇本跨您的應用程式維持您的品牌的外觀
- 影響所有的視覺狀態、 動畫和不透明度變化，而不需要重新範本

### <a name="how-to-use-colorschemeresources"></a>如何使用 ColorSchemeResources

ColorSchemeResources 是 API，告知哪些資源正在進行的系統範圍的位置。 必須採取 ColorSchemeResources [X:key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute)，這可以是三個選項的其中一個：
- 預設值
  * 會顯示您的色彩變更，在這兩[Light](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)並[深色](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)佈景主題
- 亮
  * 會顯示您的色彩變更只有在[淺色佈景主題](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) 
- 暗
  * 會顯示您的色彩變更只有在[暗色調佈景主題](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)

設定該 x： 機碼可確保您的色彩適當地變更為 「 系統或應用程式的佈景主題，如果您想在其中一個主題中的不同自訂外觀。

### <a name="how-to-apply-scoped-colors"></a>如何套用已設定領域的色彩

設定透過資源的範圍**ColorSchemeResources** XAML 中的 API 可讓您採取任何系統色彩或中的筆刷我們[佈景主題資源](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources)程式庫和重新定義這些範圍內的頁面或容器。

比方說，如果您定義兩種系統色彩- **SystemBaseLowColor**和**SystemBaseMediumLowColor**在方格中，並在頁面上放置兩個按鈕： 一個在該方格中和一個外部：

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default" 
        SystemBaseLowColor="LightGreen" 
        SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

您會收到**Button_A**套用新的色彩，和**Button_B**仍將是像我們的系統預設按鈕的外觀：

![在按鈕上的已設定領域的系統色彩](images/color/scopedcolors_cyan_button.png)

不過，由於我們所有的系統色彩重疊到其他控制項，設定**SystemBaseLowColor**並**SystemBaseMediumLowColor**會影響不只是按鈕。 控制項，在此情況下，例如**ToggleButton**， **RadioButton**並**滑桿**也會受到影響這些系統色彩的變更，應該這些控制項放在上面 exampl方格的範圍。
如果您想要設定的系統色彩變更的範圍*單一控制項僅*則可以藉由定義**ColorSchemeResources**該控制項的資源內：

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorSchemeResources x:Key="Default" 
                SystemBaseLowColor="LightGreen" 
                SystemBaseMediumLowColor="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
您基本上需要完全一樣，但現在加入方格中的任何其他控制項卻無法收取色彩變更。 這是因為這些系統色彩的範圍限於**Button_A**只。

### <a name="nesting-scoped-resources"></a>巢狀範圍資源

巢狀系統色彩也是可行的並會完成藉由將放**ColorSchemeResources**在您的應用程式版面配置的標記內的巢狀項目的資源：

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default"
            SystemBaseLowColor="LightGreen"
            SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorSchemeResources x:Key="Default"
                SystemBaseLowColor="Goldenrod"
                SystemBaseMediumLowColor="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

在此範例中， **Button_A**中會定義繼承的色彩**Grid_A**的資源，並**巢狀按鈕**繼承色彩**Grid_B**的資源。 延伸模組，這表示任何其他控制項置於**Grid_B**就會檢查或套用**Grid_B**的資源前，之前檢查或套用**Grid_A**的資源和最後套用我們的預設色彩，如果沒有任何頁面或應用程式層級定義。

這適用於任何數目的巢狀項目，其資源有色彩定義。

### <a name="scoping-with-a-resourcedictionary"></a>使用 ResourceDictionary 範圍

您不一定要將容器或頁面的資源，並可以合併在任何範圍的方式，您通常會合併字典的 ResourceDictionary 中也可以定義這些系統色彩。

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

首先，您會建立 ResourceDictionary。 然後將放置**ColorPaletteResources** ThemeDictionaries 中，並覆寫所需的系統色彩：

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TestApp">

    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
            <ResourceDictionary.MergedDictionaries>
            
                <ColorPaletteResources x:Key="Default"
                    Accent="#FF0073CF" 
                    AltHigh="#FF000000" 
                    AltLow="#FF000000"/>
                    
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>        
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

#### <a name="mainpagexaml"></a>MainPage.xaml

在包含您的版面配置頁面上，只要合併該字典中，在您想要的範圍：

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <ResourceDictionary Source="MyCustomTheme.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
    </Grid.Resources>
             
    <Button Content="Button_A"/>
</Grid>
```

現在，在單一放置所有資源、 佈景主題和自訂色彩**MyCustomTheme**資源字典和範圍視而不必擔心配置標記中的額外雜亂。

### <a name="other-ways-to-define-color-resources"></a>其他方式來定義色彩資源

ColorSchemeResources 也可讓系統要放置的色彩和的包裝函式，而不在列中，直接在其中定義：

``` xaml
<ColorSchemeResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorSchemeResources>
```

## <a name="usability"></a>可用性

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>相關文章

- [XAML 樣式](../controls-and-patterns/xaml-styles.md)
- [XAML 佈景主題資源](../controls-and-patterns/xaml-theme-resources.md)
