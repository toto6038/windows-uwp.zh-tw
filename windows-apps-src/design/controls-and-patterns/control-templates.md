---
author: Jwmsft
Description: You can customize a control's visual structure and visual behavior by creating a control template in the XAML framework.
MS-HAID: dev\_ctrl\_layout\_txt.control\_templates
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 控制項範本
ms.assetid: 6E642626-A1D6-482F-9F7E-DBBA7A071DAD
label: Control templates
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ae344e9f10c5d1dbfd530950851e402da4bc2a0d
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "5433497"
---
# <a name="control-templates"></a>控制項範本

 

您可以藉由在 XAML 架構中建立控制項範本，自訂控制項的視覺結構和視覺行為。 控制項有許多屬性 (例如 [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395)、[**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) 及 [**FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209404))，您可以設定它們來指定控制項外觀的不同方面。 但是可以透過設定這些屬性來進行的變更有限。 您可以使用 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 類別來建立範本，以指定其他自訂項目。 這裡為您示範如何建立 **ControlTemplate** 來自訂 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 控制項的外觀。

> **重要 API**：[**ControlTemplate 類別**](https://msdn.microsoft.com/library/windows/apps/br209391)、[**Control.Template 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.template.aspx)


## <a name="custom-control-template-example"></a>自訂控制項範本的範例


根據預設，[**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 控制項會將其內容 (**CheckBox** 旁邊的字串或物件) 放到選取方塊的右側，而核取記號則表示使用者已選取 **CheckBox**。 這些特性代表 **CheckBox** 的視覺結構和視覺行為。

以下是使用預設 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209316) 的 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209391) 在 `Unchecked`、`Checked` 以及 `Indeterminate` 狀態時所顯示的樣子。

![預設的 CheckBox 範本](images/templates-checkbox-states-default.png)

您可以為 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209391) 建立 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209316) 來變更這些特性。 舉例來說，如果您希望核取方塊的內容出現在選取方塊的下方，而且想要使用 **X** 來表示使用者已選取核取方塊。 可以在 **CheckBox** 的 **ControlTemplate** 中變更這些特性。

若要使用自訂範本搭配控制項，請將 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 指派給控制項的 [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465) 屬性。 以下是使用名為 `CheckBoxTemplate1` 之 **ControlTemplate** 的 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)。 下一節將會說明 **ControlTemplate** 的 Extensible Application Markup Language (XAML)。

```XAML
<CheckBox Content="CheckBox" Template="{StaticResource CheckBoxTemplate1}" IsThreeState="True" Margin="20"/>
```

以下是套用範本之後，[**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 在 `Unchecked`、`Checked` 以及 `Indeterminate` 狀態下的外觀。

![自訂核取方塊的範本](images/templates-checkbox-states.png)

## <a name="specify-the-visual-structure-of-a-control"></a>指定控制項的視覺結構


在建立 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 時，您可以合併各個 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 物件來建置單一控制項。 **ControlTemplate** 只能有一個 **FrameworkElement** 做為其根元素。 根元素通常包含其他 **FrameworkElement** 物件。 物件組合起來就形成控制項的視覺結構。

這個 XAML 為 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209391) 建立一個 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209316)，指出控制項的內容位於選取方塊的下方。 根元素是 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250)。 此範例指定 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 建立 **X**，指出使用者已選取 **CheckBox**，並建立 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 指出尚未決定的狀態。 請注意，**Path** 與 **Ellipse** 的 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 都是設定為 0，因此預設兩者都不會顯示。

[TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) 是特殊的繫結，這會將控制項範本中的屬性值連結至範本化控制項上一些其他已公開屬性的值。 TemplateBinding 只能在 XAML 的 ControlTemplate 定義中使用。 如需詳細資訊，請參閱 [TemplateBinding 標記延伸](../../xaml-platform/templatebinding-markup-extension.md)。

> [!NOTE]
> 從開始到 Windows 10 的下一個主要更新，您可以使用[**X:bind**](https://msdn.microsoft.com/library/windows/apps/Mt204783)標記延伸的位置，您使用[TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md)。 如需詳細資訊，請參閱 [TemplateBinding 標記延伸](../../xaml-platform/templatebinding-markup-extension.md)。

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}" 
            BorderThickness="{TemplateBinding BorderThickness}" 
            Background="{TemplateBinding Background}">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20" 
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}" 
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph" 
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z" 
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                  FlowDirection="LeftToRight" 
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph" 
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter" 
                              ContentTemplate="{TemplateBinding ContentTemplate}" 
                              Content="{TemplateBinding Content}" 
                              Margin="{TemplateBinding Padding}" Grid.Row="1" 
                              HorizontalAlignment="Center" 
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

## <a name="specify-the-visual-behavior-of-a-control"></a>指定控制項的視覺行為


視覺行為會指定控制項處於特定狀態時的外觀。 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 控制項有 3 種核取狀態：`Checked`、`Unchecked` 以及 `Indeterminate`。 [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/br209798) 屬性的值決定 **CheckBox** 的狀態，而其狀態決定方塊中顯示的項目。

這個表格列出 [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/br209798) 的可能值、[**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 的對應狀態，以及 **CheckBox** 的外觀。

|                     |                    |                         |
|---------------------|--------------------|-------------------------|
| **IsChecked** 值 | **CheckBox** 狀態 | **CheckBox** 外觀 |
| **true**            | `Checked`          | 包含一個 "X"。        |
| **false**           | `Unchecked`        | 空白。                  |
| **null**            | `Indeterminate`    | 包含一個圓形。      |

 

您可以使用 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) 物件來指定控制項處於特定狀態時的外觀。 **VisualState** 包含可變更 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br208817) 中元素外觀的 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br243053) 或 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br209391)。 當控制項進入 [**VisualState.Name**](https://msdn.microsoft.com/library/windows/apps/br209031) 屬性指定的狀態時，會套用 **Setter** 或 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) 中的屬性變更。 當控制項結束該狀態，變更就會移除。 將 **VisualState** 物件新增至 [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014) 物件。 將 **VisualStateGroup** 物件新增至 [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505) 附加屬性 (您可以在 **ControlTemplate** 的根 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) 上設定)。

下列 XAML 顯示 `Checked`、`Unchecked` 以及 `Indeterminate` 狀態的 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) 物件。 此範例會設定在 [**Border**](https://msdn.microsoft.com/library/windows/apps/hh738505) ([**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209250) 的根元素) 上的 [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/br209391) 附加屬性。 `Checked` **VisualState** 指定 [**Path**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) (名為 `CheckGlyph`，如前述範例所示) 的 [**Opacity**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 是 1。 `Indeterminate` **VisualState** 指定 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) (名為 `IndeterminateGlyph`) 的 **Opacity** 是 1。 `Unchecked` **VisualState** 沒有 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 或 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)，因此 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 會回到其預設外觀。

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}" 
            BorderThickness="{TemplateBinding BorderThickness}" 
            Background="{TemplateBinding Background}">
            
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CheckStates">
                <VisualState x:Name="Checked">
                    <VisualState.Setters>
                        <Setter Target="CheckGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1" 
                         Storyboard.TargetName="CheckGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
                <VisualState x:Name="Unchecked"/>
                <VisualState x:Name="Indeterminate">
                    <VisualState.Setters>
                        <Setter Target="IndeterminateGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="IndeterminateGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20" 
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}" 
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph" 
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z" 
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                  FlowDirection="LeftToRight" 
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph" 
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}" 
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter" 
                              ContentTemplate="{TemplateBinding ContentTemplate}" 
                              Content="{TemplateBinding Content}" 
                              Margin="{TemplateBinding Padding}" Grid.Row="1" 
                              HorizontalAlignment="Center" 
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

若要更加了解 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) 物件如何運作，請思考一下 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 從 `Unchecked` 狀態到 `Checked` 狀態，再到 `Indeterminate` 狀態，然後回到 `Unchecked` 狀態時，各發生什麼事。 以下為轉換情形。

|                                      |                                                                                                                                                                                                                                                                                                                                                |                                                   |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| 狀態轉換                     | 發生什麼事                                                                                                                                                                                                                                                                                                                                   | 轉換完成時 CheckBox 的外觀 |
| 從 `Unchecked` 到 `Checked`。       | 會套用 `Checked` [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br208817) 的 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br209007) 值，所以 `CheckGlyph` 的 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 為 1。                                                                                                                                                         | 顯示 X。                                |
| 從 `Checked` 到 `Indeterminate`。   | 會套用 `Indeterminate` [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br208817) 的 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br209007) 值，所以 `IndeterminateGlyph` 的 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 為 1。 會移除 `Checked` **VisualState** 的 **Setter** 值，所以 `CheckGlyph` 的 [**Opacity**](https://msdn.microsoft.com/library/windows/apps/br228078) 為 0。 | 顯示圓形。                            |
| 從 `Indeterminate` 到 `Unchecked`。 | 會移除 `Indeterminate` [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br208817) 的 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br209007) 值，所以 `IndeterminateGlyph` 的 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 為 0。                                                                                                                                           | 沒有顯示任何東西。                             |

 
如需有關如何建立控制項視覺狀態的詳細資訊，特別是如何使用 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) 類別和動畫類型，請參閱[視覺狀態的腳本動畫](https://msdn.microsoft.com/library/windows/apps/xaml/jj819808)。

## <a name="use-tools-to-work-with-themes-easily"></a>使用工具輕鬆處理主題

快速將樣式套用到控制項的方法，就是在 Microsoft Visual Studio **線上文件**的控制項上按一下滑鼠右鍵，然後選取**編輯佈景主題**或**編輯樣式** \(依您用滑鼠右鍵按一下的控制項而定\)。 然後您可以選取 \[**套用資源**\] 來套用現有的佈景主題，或選取 \[**建立空白**\] 來定義新的佈景主題。

## <a name="controls-and-accessibility"></a>控制項和協助工具

當您建立控制項的新範本時，除了可能變更控制項的行為和視覺外觀，還可能變更控制項本身在協助工具架構的表現方式。 通用 Windows 平台 (UWP) 支援 Microsoft 使用者介面自動化的協助工具架構。 所有預設控制項和其範本都支援適用於控制項的目的和功能的一般 UI 自動化控制項類型和模式。 這些控制項類型和模式都由 UI 自動化用戶端 (例如，輔助技術) 解譯，而且這可以讓控制項做為大型無障礙應用程式 UI 的一部分加以存取。

為了區隔基本的控制項邏輯，同時滿足一些 UI 自動化的架構需求，控制項類別會在獨立的類別 (自動化對等) 中包含協助工具支援。 自動化對等有時會與控制項範本互動，因為對等預期範本中會有某些已命名的部分，因此可能會有啟用輔助技術來叫用按鈕動作這類功能。

當您建立全新的自訂控制項時，有時也會想要同時建立新的自動化對等。 如需詳細資訊，請參閱[自訂自動化對等](../accessibility/custom-automation-peers.md)。

## <a name="learn-more-about-a-controls-default-template"></a>深入了解控制項的預設範本

記載 XAML 控制項樣式和範本的主題顯示了一些 XAML 摘錄，這些摘錄與您使用先前說明的 **「編輯佈景主題」** 或 **「編輯樣式」** 技術時會看到的起始 XAML 摘錄相同。 每一個主題都會列出視覺狀態的名稱、所使用的佈景主題資源，以及包含範本之完整樣式的 XAML。 如果您已經開始修改範本而且想要看看原始範本的樣子，或想確認您的新範本是否具備所有必要的指定視覺狀態，這些主題可做為有用的指引。

## <a name="theme-resources-in-control-templates"></a>控制項範本中的佈景主題資源

針對 XAML 範本中的某些屬性，您可能已經注意到使用 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)的資源參考。 這是一種技術，可讓單一控制項範本根據目前使用中的佈景主題來使用不同值的資源。 這對筆刷和色彩而言特別重要，因為佈景主題的主要目的就是讓使用者選擇套用到整體系統的深、淺或高對比佈景主題。 使用 XAML 資源系統的應用程式可以使用適用於該佈景主題的資源集，所以應用程式 UI 中的佈景主題選擇可以反映使用者的全系統佈景主題選擇。

 ## <a name="get-the-sample-code"></a>取得範例程式碼
* [XAML UI 基本知識範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [自訂文字編輯控制項範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomEditControl)

 



