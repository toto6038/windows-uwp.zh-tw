---
description: 了解如何在 UWP 應用程式中使用輔色及佈景主題。
title: UWP 應用程式中的色彩
ms.date: 04/07/2019
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5f040060b1c3931e9ef1634fddd65febb1be7dbc
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867721"
---
# <a name="color"></a>色彩

![主角圖像](images/header-color.svg)

色彩可提供直覺的溝通資訊方式給您應用程式中的使用者：色彩可用來指出互動性、提供意見反應給使用者動作，以及讓您的介面產生視覺延續性。

在 UWP 應用程式中，色彩主要由輔色及佈景主題來決定。 在本文中，我們會討論如何在應用程式中使用色彩，以及如何使用輔色及佈景主題資源，讓您的 UWP 應用程式可用於任何佈景主題背景。

## <a name="color-principles"></a>色彩原則

:::row:::
    :::column:::
**以有意義的方式使用色彩。**
謹慎使用色彩來醒目提示重要元素，可以協助建立流暢、直覺的使用者介面。
    :::column-end:::
    :::column:::
**使用色彩來表示互動性。**
最好選擇一種色彩來表示您應用程式中的可互動元素。 例如，許多網頁使用藍色文字來表示超連結。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
**色彩是個人的。**
在 Windows 中，使用者可以選擇在他們的體驗中要反映的輔色和淺色或深色佈景主題。 您可以選擇如何將使用者的輔色及佈景主題整合到應用程式中，以提供個人化的體驗。
    :::column-end:::
    :::column:::
**色彩是與文化有關的。**
請考慮您使用的色彩會如何被來自不同文化背景的人解讀。 例如，在某些文化中，藍色代表美德和保護，但在其他文化中代表服喪。
    :::column-end:::
:::row-end:::

## <a name="themes"></a>佈景主題

UWP 應用程式可以使用淺色或深色應用程式佈景主題。 佈景主題會影響應用程式的背景、文字、圖示和[通用控制項](../controls-and-patterns/index.md)。

### <a name="light-theme"></a>淺色佈景主題

![淺色佈景主題](images/color/light-theme.svg)

### <a name="dark-theme"></a>深色佈景主題

![深色佈景主題](images/color/dark-theme.svg)

根據預設，您的 UWP 應用程式佈景主題是 Windows 設定中使用者的佈景主題喜好設定，或裝置的預設佈景主題 (也就是 XBox 上的深色)。 不過，您可以設定適用於 UWP 應用程式的佈景主題。

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
建立自訂控制項的範本時，請使用佈景主題筆刷，而非硬式編碼色彩值。 如此一來，您的應用程式可以輕鬆地適應任何佈景主題。

例如，這些 [ListView 的項目範本](../controls-and-patterns/item-templates-listview.md)會示範如何在自訂範本中使用主題筆刷。
    :::column-end:::
    :::column:::
 ![具有圖示的雙行清單項目範例](images/color/list-view.svg)
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
![使用者選取的強調標題](images/color/user-accent.svg)
 ![使用者選取的輔色](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
![自訂強調標題](images/color/custom-accent.svg)
![自訂品牌輔色](images/color/brand-color.svg)
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

## <a name="scoping-system-colors"></a>界定系統色彩的範圍

除了在應用程式中定義您自己的色彩之外，您也可以使用 **ColorPaletteResources** 標記，將系統化色彩的範圍設定到整個應用程式上任何想要的區域。 此 API 不僅可讓您透過少數幾個屬性設定，同時為大量控制項設定色彩及佈景主題，還提供了一般無法透過手動定義自訂色彩取得的系統優勢：

- 使用 **ColorPaletteResources** 設定的任何色彩不會影響高對比
  * 這表示不需要任何其他設計或開發成本，就可讓您的應用程式更平易近人
- 藉由在 API 上設定一個屬性，即可輕鬆地將色彩設定為淺色、暗色或這兩個佈景主題上的普遍色彩
- **ColorPaletteResources** 上設定的色彩會套用到所有也使用該系統色彩的類似控制項
  * 在您維護品牌外觀時，這可確保您應用程式有一致的色彩內容
- 可影響所有的視覺狀態、動畫和不透明度變化，且無須重新製作範本

### <a name="how-to-use-colorpaletteresources"></a>如何使用 ColorPaletteResources

ColorPaletteResources 是一項 API，可將資源的範圍位置告知系統。 ColorPaletteResources 必須使用 [X:key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute)，而這可以是三個選項的其中一個：
- 預設值
  * 在[淺色](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)和[深色](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)佈景主題中顯示您的色彩變更
- 亮
  * 只在[淺色佈景主題](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)中顯示您的色彩變更
- 暗
  * 只在[暗色佈景主題](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)中顯示您的色彩變更

如果您想在其中一個主題中有不同的自訂外觀，設定該 x:Key 可確保您的色彩適當地變更為系統或應用程式的佈景主題。

### <a name="how-to-apply-scoped-colors"></a>如何套用已設定範圍的色彩

透過 XAML 中的 **ColorPaletteResources** API 設定資源範圍，可讓您取用我們[佈景主題資源庫](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources)中的任何系統色彩和筆刷，並可在頁面或容器的範圍內將其重新定義。

例如，如果您已在方格中定義兩種系統色彩 - **BaseLow** 和 **BaseMediumLow**，然後在頁面上放置兩個按鈕：一個在該方格內，一個在該方格外：

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
        BaseLow="LightGreen"
        BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

**Button_A** 會是套用的新色彩，而 **Button_B** 仍看起來像是我們的系統預設按鈕：

![按鈕上已設定範圍的系統色彩](images/color/scopedcolors_cyan_button.png)

不過，由於我們所有的系統色彩都會套用到其他控制項，設定 **BaseLow** 和 **BaseMediumLow** 將會影響到按鈕以外的項目。 在此案例中，如果 **ToggleButton**、**RadioButton** 和 **Slider** 等控制項放在上述範例方格的範圍內，則也會受到這些系統色彩變更的影響。
如果您想要將系統色彩變更的範圍設定為「僅限單一控制項」  ，可以藉由在該控制項的資源內定義 **ColorPaletteResources**，來達到此目的：

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="LightGreen"
                BaseMediumLow="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
所做的事情基本上跟之前完全一樣，但現在加入方格中的任何其他控制項將無法接收色彩變更。 這是因為這些系統色彩的範圍只限於 **Button_A**。

### <a name="nesting-scoped-resources"></a>巢狀範圍資源

以巢狀架構設定系統色彩也是可行的，只要將 **ColorPaletteResources** 放在您應用程式版面配置標記內的巢狀元素資源中即可：

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
            BaseLow="LightGreen"
            BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="Goldenrod"
                BaseMediumLow="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

在此範例中，**Button_A** 中會繼承 **Grid_A** 資源中定義的色彩，而**巢狀按鈕**會繼承 **Grid_B** 資源中的色彩。 因此，這表示放在 **Grid_B** 的任何其他控制項會先檢查或套用 **Grid_B** 的資源，然後再檢查或套用 **Grid_A** 的資源，如果頁面或應用程式層級上未定義任何色彩，最後就會套用我們的預設色彩。

這適用於任何數目的巢狀項目，只要其資源有色彩定義即可。

### <a name="scoping-with-a-resourcedictionary"></a>ResourceDictionary 的範圍

除了容器或頁面的資源外，您也可以在 ResourceDictionary 中定義這些系統色彩，然後再以平常合併字典的方式在任何範圍上將其合併。

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

首先，您會建立 ResourceDictionary。 然後將 **ColorPaletteResources** 放置在 ThemeDictionaries 中，並覆寫目標系統色彩：

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

在包含您版面配置的頁面上，直接在您需要的範圍上合併該字典：

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

現在，所有資源、佈景主題和自訂色彩都可放在單一 **MyCustomTheme** 資源字典上，並且可設定任何需要的範圍，您不必擔心配置標記中會有其他雜亂的資訊。

### <a name="other-ways-to-define-color-resources"></a>其他定義色彩資源的方式

ColorPaletteResources 也允許以包裝函式的形式 (不是線性函式) 直接在自身中放置系統色彩及進行定義：

``` xaml
<ColorPaletteResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorPaletteResources>
```

## <a name="usability"></a>可用性

:::row:::
    :::column:::
![對比圖](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
**對比**

請確定不論輔色或主題為何，元素和影像都有足夠的對比來讓人區別這些項目。

考慮要在應用程式中使用的色彩時，協助工具應是主要考慮。 請使用下列指導方針，確保應用程式可盡可能供越多使用者存取。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
![對比圖](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
**光源**

請留意，環境光源中的變化可能會影響應用程式的可用性。 例如，可能會因為螢幕反光而無法在外閱讀背景為黑色的頁面，而在黑暗的房間查看背景為白色的頁面則可能會很痛苦。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
![對比圖](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
**色盲**

請留意，色盲可能會影響您應用程式的可用性。 例如，紅綠色盲的使用者將難以區別紅色和綠色元素。 大約 **8% 的**男性**和 0.5% 的**女性是紅綠色盲，因此請避免將這些色彩組合用做為應用程式元素之間的唯一區別方法。
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>相關文章

- [XAML 樣式](../controls-and-patterns/xaml-styles.md)
- [XAML 佈景主題資源](../controls-and-patterns/xaml-theme-resources.md)
