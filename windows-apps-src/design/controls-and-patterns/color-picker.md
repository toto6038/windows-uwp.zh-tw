---
description: 了解如何使用色彩選擇器，讓使用者瀏覽並選取色彩，或指定 RGB、HSV 或十六進位格式的色彩。
title: 色彩選擇器
label: Color Picker
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 886236000c31dbeda12ae95e4c920be0a2cb03f3
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750554"
---
# <a name="color-picker"></a>色彩選擇器

色彩選擇器可用來瀏覽和選取色彩。 預設狀態下，使用者可在色彩頻譜上瀏覽各種顏色，或以 [紅綠藍 (RGB)]、[色調飽和值 (HSV)] 或 [十六進位] 文字方塊來指定色彩。

![預設色彩選擇器](images/color-picker-default.png)

**取得 Windows UI 程式庫**

:::row:::
   :::column:::
      ![WinUI 標誌](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      **ColorPicker** 控制項包含在 Windows UI 程式庫中，該程式庫是 NuGet 套件，其包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows UI 程式庫 API：** [ColorPicker 類別](/uwp/api/microsoft.ui.xaml.controls.colorpicker)、[Color 屬性](/uwp/api/microsoft.ui.xaml.controls.colorpicker.Color)、[ColorChanged 事件](/uwp/api/microsoft.ui.xaml.controls.colorpicker.ColorChanged)
>
> **平台 API：** [ColorPicker 類別](/uwp/api/windows.ui.xaml.controls.colorpicker)、[Color 屬性](/uwp/api/windows.ui.xaml.controls.colorpicker.Color)、[ColorChanged 事件](/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用色彩選擇器，即可讓使用者在您的應用程式中選取色彩。 例如，可用來變更字型色彩、背景或應用程式佈景主題色彩等色彩設定。

如果您的應用程式是專門用來繪圖，或是其他適合使用手寫筆的類似工作，建議在使用[手寫筆跡控制項](inking-controls.md)時，搭配色彩選擇器。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已經安裝了 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/ColorPicker">開啟應用程式，並查看 ColorPicker 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-color-picker"></a>建立色彩選擇器

此範例示範如何使用 XAML 來建立預設色彩選擇器。

```xaml
<ColorPicker x:Name="myColorPicker"/>
```

在預設狀態下，色彩選擇器會在色彩頻譜旁的矩形列上，預覽顯示所選色彩。 可以使用 [ColorChanged](/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged) 事件或 [Color](/uwp/api/windows.ui.xaml.controls.colorpicker.Color) 屬性來取得所選的色彩，並在應用程式中使用。 如需詳細程式碼，請參閱下列範例。

### <a name="bind-to-the-chosen-color"></a>繫結至選擇的色彩

當色彩選擇動作需要立即生效時，請使用資料繫結來繫結至 Color 屬性，或處理 ColorChanged 事件，以在程式碼中取得所選的色彩。

在此範例中，SolidColorBrush 用於做為矩形填滿色彩的 Color 屬性，會直接繫結至色彩選擇器所選的色彩。 色彩選擇器的任何變更，都會對繫結屬性產生即時變更。

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>

<Rectangle Height="50" Width="50">
    <Rectangle.Fill>
        <SolidColorBrush Color="{x:Bind myColorPicker.Color, Mode=OneWay}"/>
    </Rectangle.Fill>
</Rectangle>
```

此範例使用的是簡單的色彩選擇器，只有圓圈和滑桿，也就是常見的「隨意」挑選色彩的方式。 當色彩變更可以即時顯現在受影響的物件上時，就不需要顯示色彩預覽列。 如需詳細資訊，請參閱〈自訂色彩選擇器〉  一節。

### <a name="save-the-chosen-color"></a>儲存選擇的色彩

在某些案例中，並不需要立即套用色彩變更。 舉例來說，在飛出視窗中裝載色彩選擇器時，建議在使用者確認選取項目或關閉飛出視窗之後，再套用選取的色彩。 您也可以儲存選取的色彩值，以供稍後使用。

在此範例中，色彩選擇器是裝載於具有 [確認] 和 [取消] 按鈕的飛出視窗中。 當使用者確定所選色彩時，您要在應用程式中儲存選取的色彩，以供稍後使用。

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

並非所有的欄位都需要讓使用者挑選色彩，因此色彩選擇器很有彈性。 色彩選擇器提供了豐富的選項，可讓您配合需求來設定控制項。

舉例來說，如果使用者不需要精準控制 (例如在筆記應用程式中挑選螢光筆色彩)，就可以使用簡化版的 UI。 您可以隱藏文字輸入欄位，並將色彩頻譜變更為圓形。

當使用者確實需要精準控制時 (例如平面設計應用程式)，就可以顯示色各項特性的滑桿和文字輸入欄位。

#### <a name="show-the-circle-spectrum"></a>顯示圓形頻譜

此範例會示範如何使用 [ColorSpectrumShape](/uwp/api/windows.ui.xaml.controls.colorpicker.ColorSpectrumShape) 屬性，將色彩選擇器設為使用圓形頻譜，而非使用預設的方形頻譜。

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"/>
```

![使用圓形頻譜的色彩選擇器](images/color-picker-ring.png)

當必須選擇要使用方形還是圓形的色彩頻譜時，最主要的考量在於準確度。 使用者使用方形頻譜選取特定色彩時，因為顯示的色域圖範圍較大，使用者較能精準控制色彩。 若要讓使用者更加「隨意」地選擇色彩，建議使用圓形頻譜。

#### <a name="show-the-alpha-channel"></a>顯示 Alpha 色板

在此範例中，要啟用色彩選擇器上的透明度滑桿和文字方塊。

```xaml
<ColorPicker x:Name="myColorPicker"
             IsAlphaEnabled="True"/>
```

![IsAlphaEnabled 設定為 true 的色彩選擇器](images/color-picker-alpha.png)

#### <a name="show-a-simple-picker"></a>顯示簡易選擇器

此範例會說明如何設定採用簡易 UI 的色彩選擇器，以供「隨意」使用。 您要顯示圓形頻譜，並隱藏預設的文字輸入方塊。 當色彩變更可以即時顯現在受影響的物件上時，就不需要顯示色彩預覽列。 若非如此，就應顯示色彩預覽功能。

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>
```

![簡易色彩選擇器](images/color-picker-casual.png)

#### <a name="show-or-hide-additional-features"></a>顯示或隱藏其他功能

下表列出了可用於設定 ColorPicker 控制項的所有選項。

功能 | [內容]
--------|-----------
色彩頻譜 | IsColorSpectrumVisible、ColorSpectrumShape、ColorSpectrumComponents
色彩預覽 | IsColorPreviewVisible
色彩值| IsColorSliderVisible、IsColorChannelTextInputVisible
不透明度值 | IsAlphaEnabled、IsAlphaSliderVisible、IsAlphaTextInputVisible
十六進位值 | IsHexInputVisible

> [!NOTE]
> IsAlphaEnabled 必須為 **true**，才能顯示不透明度文字方塊或滑桿。 然後，您可以使用 IsAlphaTextInputVisible 和 IsAlphaSliderVisible 屬性，來修改輸入控制項的可見度。 如需詳細資訊，請參閱 API 文件。

## <a name="dos-and-donts"></a>可行與禁止事項

- 請多加思考您的應用程式適合採用何種色彩選擇方式。 有些情境下，可能並不需要精準地挑選色彩，因此簡易版的選擇器反而更加實用
- 如需提供最精準的色彩挑選體驗，請使用方形頻譜，並確保大小至少達 256x256px，或是加入文字輸入欄位，讓使用者可以精細調整選取的色彩。
- 在飛出視窗中使用色彩選擇器時，單單點選頻譜或調整滑桿，並不會確定選擇色彩。 如何確定選取色彩：
  - 請提供確定及取消功能的按鈕，以便套用或取消選擇。 點擊 [返回] 按鈕，或點選飛出視窗以外的地方，即會關閉視窗，但不會儲存使用者的選擇項目。
  - 或者，藉由點選飛出視窗外部或按 [返回] 按鈕，在關閉飛出視窗時認可選擇。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [Windows 應用程式中的手寫筆互動](../input/pen-and-stylus-interactions.md)
- [手寫筆跡控制項](inking-controls.md)

<!--
<div class="microsoft-internal-note">
<p>
<p>
Note: For more info, see the [color picker redlines](https://uni/DesignDepot.FrontEnd/#/ProductNav/3666/15/dv/?t=Windows%7CControls&f=RS2) on UNI.
</div>
-->
