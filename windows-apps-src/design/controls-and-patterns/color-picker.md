---
author: Jwmsft
Description: A color picker lets a user browse through and select colors.
title: 色彩選擇器
label: Color Picker
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f2b6270fcd7bc3dcf45d6e80dd547ee783d8e9ae
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6048520"
---
# <a name="color-picker"></a>色彩選擇器

色彩選擇器可用來瀏覽和選取色彩。 其預設可讓使用者瀏覽色彩頻譜上的顏色，或在 [紅綠藍 (RGB)]、[色調飽值 (HSV)] 或 [十六進位] 文字方塊中指定色彩。

> **重要 API**：[ColorPicker 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker)、[Color 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color)、[ColorChanged 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged)

![預設色彩選擇器](images/color-picker-default.png)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用色彩選擇器，讓使用者在您的 App 中選取色彩。 例如，用來變更字型色彩、背景或 App 佈景主題色彩等色彩設定。

如果您的 App 適合使用手寫筆繪圖或進行類似工作，請考慮搭配色彩選擇器來使用[筆跡控制項](inking-controls.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/ColorPicker">開啟應用程式並查看 ColorPicker 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-color-picker"></a>建立色彩選擇器

此範例示範如何使用 XAML 來建立預設色彩選擇器。

```xaml
<ColorPicker x:Name="myColorPicker"/>
```

色彩選擇器預設會在色彩頻譜旁邊的矩形列上顯示所選色彩的預覽。 您可以使用 [ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged) 事件或 [Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color) 屬性來存取選取的色彩，並在 App 中使用此色彩。 如需詳細程式碼，請參閱下列範例。

### <a name="bind-to-the-chosen-color"></a>繫結至選擇的色彩

當色彩選擇需要立即生效時，您可以使用資料繫結來繫結至 Color 屬性，或處理 ColorChanged 事件以在程式碼中存取選取的色彩。

在此範例中，您可以將 SolidColorBrush 用於做為矩形填滿色彩的 Color 屬性直接繫結至色彩選擇器的所選色彩直接矩形。 色彩選擇器的任何變更都會對繫結的屬性造成即時變更。

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape=”Ring”
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>

<Rectangle Height="50" Width="50">
    <Rectangle.Fill>
        <SolidColorBrush Color="{x:Bind myColorPicker.Color, Mode=OneWay}"/>
    </Rectangle.Fill>
</Rectangle>
```

此範例使用只有圓圈和滑桿的簡化色彩選擇器，也就是一般「隨性」的色彩挑選體驗。 當色彩變更可以即時顯現在受影響的物件時，您不需要顯示色彩預覽列。 如需詳細資訊，請參閱*自訂色彩選擇器*一節。

### <a name="save-the-chosen-color"></a>儲存選擇的色彩

在某些情況下，您並不想要立即套用色彩變更。 例如，在飛出視窗中裝載色彩選擇器時，我們會建議您只有在使用者確認選取項目或關閉飛出視窗之後，才要套用選取的色彩。 您也可以儲存選取的色彩值以供稍後使用。

在此範例中，您色彩選擇器裝載於具有 [確認] 和 [取消] 按鈕的飛出視窗中。 當使用者確認其色彩選擇時，您會在 App 中儲存選取的色彩以供稍後使用。

```xaml
<Page.Resources>
    <Flyout x:Key="myColorPickerFlyout">
        <RelativePanel>
            <ColorPicker x:Name="myColorPicker"
                         IsColorChannelTextInputVisible="False"
                         IsHexInputVisible="False"/>

            <Grid RelativePanel.Below="myColorPicker"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Content="OK" Click="confirmColor_Click"
                        Margin="0,12,2,0" HorizontalAlignment="Stretch"/>
                <Button Content="Cancel" Click="cancelColor_Click"
                        Margin="2,12,0,0" HorizontalAlignment="Stretch"
                        Grid.Column="1"/>
            </Grid>
        </RelativePanel>
    </Flyout>
</Page.Resources>

<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Button x:Name="colorPickerButton"
            Content="Pick a color"
            Flyout="{StaticResource myColorPickerFlyout}"/>
</Grid>
```

```csharp
private Color mycolor;

private void confirmColor_Click(object sender, RoutedEventArgs e)
{
    // Assign the selected color to a variable to use outside the popup.
    myColor = myColorPicker.Color;

    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}

private void cancelColor_Click(object sender, RoutedEventArgs e)
{
    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}
```

### <a name="configure-the-color-picker"></a>設定色彩選擇器

並非所有的欄位都需要讓使用者挑選色彩，因此色彩選擇器很有彈性。 其中提供各種讓您配合個人需求設定控制項的不同選項。

例如，當使用者不需要精準控制時 (像是在做筆記的 App 中挑選螢光筆色彩時)，您可以使用簡化的 UI。 您可以隱藏文字輸入欄位，並將色彩頻譜變更為圓形。

當使用者確實需要精準控制時 (像是在圖形設計 App 中使用時)，您可以針對色彩的各方面特性顯示滑桿及文字輸入欄位。

#### <a name="show-the-circle-spectrum"></a>顯示圓形頻譜

此範例示範如何使用 [ColorSpectrumShape](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorSpectrumShape) 屬性，將色彩選擇器設定為使用圓形頻譜，而不使用預設的方形頻譜。

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"/>
```

![使用圓形頻譜的色彩選擇器](images/color-picker-ring.png)

當您必須在方形和圓形色彩頻譜之間選擇時，主要的考量是準確度。 當使用者使用方形頻譜選取特定色彩時，使用者因為顯示的色域圖範圍較大而對色彩控制得更好 若要提供更濃厚的「隨性」色彩選擇體驗，您應該考慮使用圓形頻譜。

#### <a name="show-the-alpha-channel"></a>顯示 Alpha 色板

在此範例中，您可以色彩選擇器上啟用透明度滑桿和文字方塊。

```xaml
<ColorPicker x:Name="myColorPicker"
             IsAlphaEnabled="True"/>
```

![IsAlphaEnabled 設定為 true 的色彩選擇器](images/color-picker-alpha.png)

#### <a name="show-a-simple-picker"></a>顯示簡單選擇器

此範例示範如何設定包含簡單 UI 的色彩選擇器，以便「隨性」使用。 您會顯示圓形頻譜，並隱藏預設輸入方塊。 當色彩變更可以即時顯現在受影響的物件時，您不需要顯示色彩預覽列。 否則應該讓色彩預覽顯示。

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>
```

![簡單色彩選擇器](images/color-picker-casual.png)

#### <a name="show-or-hide-additional-features"></a>顯示或隱藏其他功能

下表顯示所有可用來設定 ColorPicker 控制項的選項。

功能 | 屬性
--------|-----------
色彩頻譜 | IsColorSpectrumVisible、ColorSpectrumShape、ColorSpectrumComponents
色彩預覽 | IsColorPreviewVisible
色彩值| IsColorSliderVisible、IsColorChannelTextInputVisible
不透明度值 | IsAlphaEnabled、IsAlphaSliderVisible、IsAlphaTextInputVisible
十六進位值 | IsHexInputVisible

> [!NOTE]
> IsAlphaEnabled 必須為 **true**，才能顯示不透明度文字方塊或滑桿。 然後，您可以使用 IsAlphaTextInputVisible 和 IsAlphaSliderVisible 屬性來修改輸入控制項的可見度。 如需詳細資訊，請參閱 API 文件。

## <a name="dos-and-donts"></a>可行與禁止事項

- 想想看哪一種色彩挑選體驗適合您的 App。 有些案例可能不需要精細挑選色彩，簡化的選擇器反而更實惠
- 如需最準確的色彩挑選體驗，請使用方形頻譜並確定其至少為 256x256px，或是加入文字輸入欄位，讓使用者精細調整其選取的色彩。
- 在飛出視窗中使用時，僅止是輕觸頻譜或是調整滑桿並不能認可色彩選擇。 若要認可選取的色彩：
  - 提供 [認可] 和 [取消] 按鈕，以便套用或取消選擇。 按 [返回] 按鈕 或點選飛出視窗外部會將其關閉，但不儲存使用者的選擇。
  - 或者，藉由點選飛出視窗外部或按 [返回] 按鈕，在關閉飛出視窗時認可選擇。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [UWP 應用程式中的畫筆和手寫筆互動](../input/pen-and-stylus-interactions.md)
- [筆跡](inking-controls.md)

<!--
<div class=”microsoft-internal-note”>
<p>
<p>
Note: For more info, see the [color picker redlines](http://uni/DesignDepot.FrontEnd/#/ProductNav/3666/15/dv/?t=Windows%7CControls&f=RS2) on UNI.
</div>
-->