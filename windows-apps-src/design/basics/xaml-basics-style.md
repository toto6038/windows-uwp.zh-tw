---
title: 建立自訂樣式
description: 遵循此教學課程來了解如何建立自訂樣式和滑杆控制項，以自訂 XAML 應用程式的 UI。
keywords: XAML, UWP, 開始使用
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 04357cc43ed227b109d4aab20c509077261360ee
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636598"
---
# <a name="tutorial-create-custom-styles"></a>教學課程：建立自訂樣式

本教學課程告訴您如何自訂 XAML 應用程式的 UI。 警告：本教學課程不一定會涉及獨角獸。 (確實如此！)

PhotoLab 範例應用程式有兩個頁面。 「主頁面」  會顯示影像中心檢視，以及一些關於每個影像檔案的資訊。

![Photolab 主頁面的螢幕擷取畫面。](../basics/images/xaml-basics/mainpage.png)

「詳細資料頁面」  會在選取單一相片之後，顯示該相片。 飛出視窗編輯功能表可用來變更、重新命名和儲存相片。

![Photolab 詳細頁面的螢幕擷取畫面。](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>必要條件

+ Visual Studio 2019：[下載 Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (此為免費的社群版本)。
+ Windows 10 SDK (10.0.17763.0 或更新版本)：[下載最新 Windows SDK (免費)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10，版本 1809 或更新版本

## <a name="part-0-get-the-starter-code-from-github"></a>第 0 部分：從 GitHub 取得起始程式碼

針對此教學課程，您將從簡化版的 PhotoLab 範例開始著手。

1. 移至 GitHub 範例頁面：[https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)。
2. 下一步，您將需要複製或下載範例。 選取 [複製或下載]  按鈕。 子功能表會出現。
    ![PhotoLab 範例的 GitHub 頁面上的複製或下載功能表](images/xaml-basics/clone-repo.png)

    **如果您不熟悉 GitHub：**

    a. 選取 [下載 ZIP]  ，然後在本機儲存此檔案。 這個下載的 .zip 檔案，包含您所需要的所有專案檔案。

    b. 將檔案解壓縮。 使用 [檔案總管] 瀏覽至您下載的 .zip 檔案，以滑鼠右鍵按一下檔案，然後選取 [解壓縮全部...]。

    c. 瀏覽至您範例的本機複製，並移至 `Windows-appsample-photo-lab-master\xaml-basics-starting-points\style` 目錄。

    **如果您熟悉 GitHub：**

    a. 在本機複製存放庫中的主要分支。

    b. 瀏覽至 `Windows-appsample-photo-lab\xaml-basics-starting-points\style` 目錄。

3. 按兩下 `Photolab.sln`，在 Visual Studio 中開啟解決方案。

## <a name="part-1-create-a-fancy-slider-control"></a>第 1 部分：建立美觀的滑桿控制項

Windows 應用程式提供幾種用來自訂應用程式外觀的方法。 從字型和印刷樣式設定到色彩和漸層，再到模糊效果，有許多方式可供選擇。

在本教學課程的第一個部分，讓我們來x靈活運用一些相片編輯控制項。

![使用預設樣式的平常滑桿。](../basics/images/xaml-basics/slider-start.png)

這些滑桿是不錯，滑桿該有的功能全都有，就是不太有型。 我們來修整一下。

曝光滑桿會調整影像的曝光：將它推向左邊，影像會變得較暗，推向右邊則變得較亮。 給我們的滑桿一個由黑變白的背景，使它更酷一些。 這讓滑桿變得好看多了，真的很棒，但更棒的是，還會提供關於滑桿所具功能的視覺提示。

按 F5 來編譯並執行應用程式。 第一個畫面顯示影像的圖庫。 按一下影像以移至影像詳細資料頁面。 進入後，按一下編輯按鈕以查看我們即將處理的編輯控制項。 結束應用程式，返回 Visual Studio。

### <a name="customize-a-slider-control"></a>自訂滑桿控制項

1. 在 [方案總管] 面板中，按兩下 [DetailPage.xaml]  將其開啟。

    ![Visual Studio 方案總管中的 DetailPage.xaml 檔案。](../basics/images/xaml-basics/style-detail-page-explorer.png)

1. 使用 `Polygon` 元素建立曝光滑桿的背景圖形。

    [Windows.UI.Xaml.Shapes 命名空間](/uwp/api/Windows.UI.Xaml.Shapes)提供七個可以選擇的圖形。 有橢圓形、矩形，以及所謂「路徑」的項目，這可建立任何類型的圖形；是的，甚至獨角獸都行！

    ![獨角獸](../basics/images/xaml-basics/unicorn.png)

    > **請參閱：** [繪製圖形](../controls-and-patterns/shapes.md)文章告訴您一切您需要知道關於 XAML 圖形的所有內容。

    我們想要建立三角形外觀的 Widget，就像在立體音響音量控制上看到的圖形一樣。

    ![音量滑桿](../basics/images/xaml-basics/style-volume-slider.png)

    聽起來像是 `Polygon` 形狀的工作！ 若要定義多邊形，請指定一組點並指定填滿。 我們來建立一個約 200 像素寬、20 像素高且使用漸層填滿的多邊形。

    在 DetailPage.xaml 中，尋找曝光滑桿的程式碼，然後在下列事項之前，建立 Polygon 元素：

    * 將 **Grid.Row** 設定為 "2"，將多邊形與曝光滑桿放在同一列。
    * 將 **Points** 屬性設定為 "0,20 200,20 200,0"，以定義三角形圖形。
    * 將 **Stretch** 屬性設定為 "Fill"，並將 **HorizontalAlignment** 屬性設定為 "Stretch"。
    * 將 **Height** 設定為 "20"，並將 **VerticalAlignment** 設定為 "Center"。
    * 為 **Polygon** 指定線性漸層填滿。
    * 在曝光滑桿上，將 **Foreground** 屬性設定為 "Transparent"，讓您可以看到多邊形。

    **之前**

    ```xaml
    <Slider Header="Exposure"
        Grid.Row="2"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    **之後**

    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure"
        Grid.Row="2"
        Foreground="Transparent"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    附註：
    * 如果查看 XAML 周圍，您會看到這些元素位於 `Grid` 中。 我們將多邊形放在曝光滑桿的同一列 (`Grid.Row="2"`)，因此這些元素會出現在相同位置。 我們將多邊形放在滑桿之前，讓滑桿顯示在圖形的最上方。
    * 我們會在多邊形上設定 `Stretch="Fill"` 和 `HorizontalAlignment="Stretch"`，讓三角形調整為填滿可用空間。 如果滑桿寬度變小或變大，多邊形也會縮小或擴大，與之相配合。

1. 編譯和執行應用程式。 滑桿現在看起來超棒：

    ![美觀的曝光滑桿](../basics/images/xaml-basics/style-exposure-slider-done.png)

1. 為下一個滑桿 (溫度滑桿) 進行升級。 溫度滑桿變更影像的色溫。向左推使影像更偏藍色，向右推使影像更偏黃色。

    我們將為此背景圖形使用另一個多邊形，其尺寸與前一個相同，但這次我們將填滿設定為由藍色變到黃色的漸層，而不是黑色和白色。

    **之前**

    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                 Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **之後**

    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />

    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

1. 編譯和執行應用程式。 您現在會有兩個花俏滑桿。

    ![兩個美觀的滑桿](../basics/images/xaml-basics/style-2sliders-done.png)

1. **額外加分**

    為濃淡滑桿新增具有由綠色變紅色漸層的背景圖形。

    ![3 個美觀的滑桿](../basics/images/xaml-basics/style-3sliders-done.png)

恭喜，您已完成第 1 部分！ 如果遇到困難或想要查看最終方案，您可以在 [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab) 中找到完成的程式碼。

## <a name="part-2-create-basic-styles"></a>第 2 部分：建立基本樣式

XAML 樣式的其中一個優點是，可以大幅縮減您需要撰寫的程式碼數量，而且可以讓您更輕鬆地更新應用程式外觀。

若要定義樣式，請將 [Style](/uwp/api/Windows.UI.Xaml.Style) 元素新增至包含需要設定樣式之控制項的元素的 [Resources](/uwp/api/windows.ui.xaml.frameworkelement.Resources) 屬性。  如果將樣式新增至 `Page.Resources` 屬性，整個頁面都可以存取您的樣式。 如果將樣式新增至 App.xaml 檔案中的 `Application.Resources` 屬性，整個應用程式都可以存取該樣式。

您可以建立具名樣式和一般樣式。 具名樣式必須明確套用至特定控制項；一般樣式則套用至任何符合指定之 `TargetType` 的控制項。

在這個範例中，第一個樣式具有 `x:Key` 屬性，其目標類型為 `Button`。 第一個按鈕的 `Style` 屬性設為此索引鍵，所以此樣式為具名樣式，必須明確進行套用。 第二種樣式是自動套用至第二個按鈕，因為其目標類型是 `Button`，而且該樣式並沒有 `x:Key` 屬性。

```xaml
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

讓我們將樣式新增至應用程式。 在 DetailsPage.xaml 中，看一下曝光、溫度及濃淡滑桿旁邊的文字區塊。 每個文字區塊各自顯示一個滑桿值。 以下是曝光滑桿的文字方塊。 請注意，`Margin`、`VerticalAlignment` 和 `Padding` 屬性已設定。

```xaml
<TextBlock Grid.Row="2"
            Grid.Column="1"
            Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
            Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
```

查看其他文字區塊，注意那些同樣的屬性也設定為相同的值。 聽起來好是不錯的樣式候選項目...

### <a name="create-a-value-text-block-style"></a>建立有質感的文字區塊樣式

1. 開啟 DetailsPage.xaml。

2. 尋找名為 `Grid` 的 **Grid** 控制項。 其中包含我們的滑桿和文字方塊。 請注意，方格已經為滑桿定義樣式。

    ```xaml
    <Grid x:Name="EditControlsGrid"
            HorizontalAlignment="Stretch"
            Margin="24,48,24,24">
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>
    ```

3. 為 **TextBlock** 建立一個樣式，該樣式會將 **Margin** 設為 "10,8,0,0"、將 **VerticalAlignment** 設為 [置中] 以及將 **Padding** 設為 "0"。

    **之前**

    ```xaml
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>
    ```

    **之後**

    ```xaml
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
        </Grid.Resources>
    ```

4. 讓我們將此樣式建立成具名樣式，這樣就可以指定要套用至哪些 `TextBlock` 控制項。 將樣式的 `x:Key` 屬性設定為 "ValueTextBox"。

    **之前**

    ```xaml
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

    **之後**

    ```xaml
            <Style TargetType="TextBlock"
                   x:Key="ValueTextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

5. 針對每個 `TextBlock`，移除其 `Margin`、`VerticalAlignment`和 `Padding` 屬性，並將其 `Style` 屬性設定為 "{StaticResource ValueTextBlock}"。

    **之前**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    **之後**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Style="{StaticResource ValueTextBlock}"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    對與滑桿相關聯的所有 6 個 TextBlock 控制項進行此變更。

6. 編譯和執行應用程式。 應該看起來...都一樣。 但是，您應該感受到撰寫有效率、可維護的程式碼所帶來的美妙滿足感和成就感。

恭喜，您已完成第 2 部分！

## <a name="part-3-use-a-control-template-to-make-a-fancy-slider"></a>第 3 部分：使用控制項範本建立美觀的滑桿

記得我們如何在第 1 部分新增了滑桿後面的圖形，讓它看起來更酷嗎？

嗯，工作是完成了，但還有更好的方式來達到相同的效果：建立控制項範本。

1. 在 [方案總管] 面板中，按兩下 [DetailPage.xaml]  。

1. 接下來，我們將使用滑桿的預設控制項範本做為起點。 將此 XAML 新增至 `Page.Resources` 元素。 (`Page.Resources` 元素較靠近頁面開頭)。

    此應用程式會使用 Windows UI 程式庫 (WinUI)，因此您可以從 WinUI GitHub 存放庫複製預設範本：[Slider_themeresources.xaml](https://github.com/microsoft/microsoft-ui-xaml/blob/master/dev/Slider/Slider_themeresources.xaml)。


    ```xaml
    <ControlTemplate x:Key="FancySliderControlTemplate" TargetType="Slider"
      xmlns:contract6Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract6NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract7Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,7)"
      xmlns:contract7NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,7)">
      <Grid Margin="{TemplateBinding Padding}">
        <Grid.Resources>
            <Style TargetType="Thumb" x:Key="SliderThumbStyle">
                <Setter Property="BorderThickness" Value="0" />
                <Setter Property="Background" Value="{ThemeResource SliderThumbBackground}" />
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="Thumb">
                            <Border
                                Background="{TemplateBinding Background}"
                                BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="{TemplateBinding BorderThickness}"
                                CornerRadius="{ThemeResource SliderThumbCornerRadius}" />
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </Grid.Resources>

        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState x:Name="Normal" />

                <VisualState x:Name="Pressed">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

                <VisualState x:Name="Disabled">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HeaderContentPresenter" Storyboard.TargetProperty="Foreground">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderHeaderForegroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="TopTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="BottomTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="LeftTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RightTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

                <VisualState x:Name="PointerOver">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

            </VisualStateGroup>
            <VisualStateGroup x:Name="FocusEngagementStates">
                <VisualState x:Name="FocusDisengaged" />
                <VisualState x:Name="FocusEngagedHorizontal">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
                <VisualState x:Name="FocusEngagedVertical">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

            </VisualStateGroup>

        </VisualStateManager.VisualStateGroups>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <ContentPresenter x:Name="HeaderContentPresenter"
            Grid.Row="0"
            Content="{TemplateBinding Header}"
            ContentTemplate="{TemplateBinding HeaderTemplate}"
            FontWeight="{ThemeResource SliderHeaderThemeFontWeight}"
            Foreground="{ThemeResource SliderHeaderForeground}"
            Margin="{ThemeResource SliderTopHeaderMargin}"
            TextWrapping="Wrap"
            Visibility="Collapsed"
            x:DeferLoadStrategy="Lazy"/>
        <Grid x:Name="SliderContainer"
            Grid.Row="1"
            Background="{ThemeResource SliderContainerBackground}"
            Control.IsTemplateFocusTarget="True">
            <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.RowDefinitions>
                    <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
                </Grid.RowDefinitions>

                <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="HorizontalDecreaseRect" Fill="{TemplateBinding Foreground}" Grid.Row="1"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <TickBar x:Name="TopTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    VerticalAlignment="Bottom"
                    Margin="0,0,0,4"
                    Grid.ColumnSpan="3" />
                <TickBar x:Name="HorizontalInlineTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderInlineTickBarFill}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
                <TickBar x:Name="BottomTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    VerticalAlignment="Top"
                    Margin="0,4,0,0"
                    Grid.Row="2"
                    Grid.ColumnSpan="3" />
                <Thumb x:Name="HorizontalThumb"
                    Style="{StaticResource SliderThumbStyle}"
                    DataContext="{TemplateBinding Value}"
                    Height="{ThemeResource SliderHorizontalThumbHeight}"
                    Width="{ThemeResource SliderHorizontalThumbWidth}"
                    Grid.Row="0"
                    Grid.RowSpan="3"
                    Grid.Column="1"
                    FocusVisualMargin="-14,-6,-14,-6"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
            <Grid x:Name="VerticalTemplate" MinWidth="{ThemeResource SliderVerticalWidth}" Visibility="Collapsed">

                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="{ThemeResource SliderPreContentMargin}" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="{ThemeResource SliderPostContentMargin}" />
                </Grid.ColumnDefinitions>

                <Rectangle x:Name="VerticalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="VerticalDecreaseRect"
                    Fill="{TemplateBinding Foreground}"
                    Grid.Column="1"
                    Grid.Row="2"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <TickBar x:Name="LeftTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    HorizontalAlignment="Right"
                    Margin="0,0,4,0"
                    Grid.RowSpan="3" />
                <TickBar x:Name="VerticalInlineTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderInlineTickBarFill}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3" />
                <TickBar x:Name="RightTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    HorizontalAlignment="Left"
                    Margin="4,0,0,0"
                    Grid.Column="2"
                    Grid.RowSpan="3" />
                <Thumb x:Name="VerticalThumb"
                    Style="{StaticResource SliderThumbStyle}"
                    DataContext="{TemplateBinding Value}"
                    Width="{ThemeResource SliderVerticalThumbWidth}"
                    Height="{ThemeResource SliderVerticalThumbHeight}"
                    Grid.Row="1"
                    Grid.Column="0"
                    Grid.ColumnSpan="3"
                    FocusVisualMargin="-6,-14,-6,-14"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
        </Grid>
      </Grid>
    </ControlTemplate>
    ```

    哇，好多 XAML 啊！ 控制項範本是非常強大的功能，但可能會相當複雜，這也就是為什麼通常要從預設範本開始著手的原因。

1. 在您剛新增的 `ControlTemplate` 中，尋找名為 `HorizontalTemplate` 的方格。 這個方格定義我們想要變更範本的部分。

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
        </Grid.RowDefinitions>
    ```

1.  建立多邊形，就像您在第 1 部分為曝光滑桿建立的多邊形一樣。 在結束 `Grid.RowDefinitions` 標籤後面新增多邊形。 將 `Grid.Row` 設定為 "0"、將 `Grid.RowSpan` 設定為 "3"，並將 `Grid.ColumnSpan` 設定為 "3"。

    **之前**

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
    ```

    **之後**

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>
    ```

1. 移除 `Polygon.Fill` 設定。 將 `Fill` 設定為 `"{TemplateBinding Background}"`。 這樣設定後，設定滑桿的 `Background` 屬性就會設定多邊形的 `Fill` 屬性。

    **之前**

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>
    ```

    **之後**

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20"
                    Fill="{TemplateBinding Background}">
        </Polygon>
    ```

1. 就在您新增的多邊形後面，有一個名為 `HorizontalTrackRect` 的矩形。 移除矩形的 `Fill` 設定，讓矩形無法顯示，以免擋住我們的多邊形圖形。 (我們並不想要完全移除矩形，因為控制項範本還會用它來產生互動視覺效果，例如滑鼠指標暫留。)

    **之前**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    **之後**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    您把範本做好了！ 現在需要將它套用至我們的滑桿。

1. 這就來更新我們的曝光滑桿。

    * 將滑杆的 `Template`屬性設定為 `"{StaticResource FancySliderControlTemplate}"`。
    * 移除滑杆的 `Background="Transparent"` 設定。
    * 將滑桿的 `Background` 設定為由黑色到白色的線性漸層。
    * 移除我們在第 1 部分建立的背景多邊形。

    **之前**

    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure"
            Grid.Row="2" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2" />
    ```

    **之後**

    ```xaml
    <Slider Header="Exposure"
            Grid.Row="2"  Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. 對溫度滑桿進行同樣的更新。

    **之前**

    ```xaml
    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **之後**

    ```xaml
    <Slider Header="Temperature"
            Grid.Row="3" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. 對濃淡滑桿進行同樣的更新。

    **之前**

    ```xaml
    <Polygon Grid.Row="4" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Tint"
            Grid.Row="4" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **之後**

    ```xaml
    <Slider Header="Tint"
            Grid.Row="4" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. 編譯和執行應用程式。

    ![世界上最好的滑桿](../basics/images/xaml-basics/style-sliders-templates.png)

    如您所見，我們的更新改善了多邊形的置放方式；現在多邊形的底部已對齊滑桿縮圖的底部。

恭喜，您已經完成教學課程了！ 如果遇到困難，而且想要查看最終方案，您可以在 [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab) 中找到完整的範例。
