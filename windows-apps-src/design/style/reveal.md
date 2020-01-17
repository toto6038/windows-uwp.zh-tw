---
description: Reveal 是一種光源效果，可協助為應用程式的互動元素帶來深度和焦點。
title: 顯示顯目提示
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 63a7ee8550b72356199645f54b587480275c2bcd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685086"
---
# <a name="reveal-highlight"></a>顯示顯目提示

![主角圖像](images/header-reveal-highlight.svg)

顯示顯目提示是會在使用者將指標移近互動式元素 (例如命令列) 時，醒目提示該元素的光源效果。 

> **重要 API**：[RevealBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush) \(英文\)、[RevealBackgroundBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush) \(英文\)、[RevealBorderBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush) \(英文\)、[RevealBrushHelper 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper) \(英文\)、[VisualState 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) \(英文\)

## <a name="how-it-works"></a>運作方式
顯示顯目提示會在指標移近互動式元素時顯示該元素的容器，藉以引起使用者對該元素的注意，如下圖所示：

![Reveal 視覺效果](images/Nav_Reveal_Animation.gif)

透過公開物件四周的隱藏邊框，Reveal 能讓使用者更進一步了解他們所互動的空間，並協助他們了解可用的動作。 這對清單控制項及按鈕群組來說尤其重要。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下此處以<a href="xamlcontrolsgallery:/item/Reveal">開啟應用程式並查看 Reveal 的運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="how-to-use-it"></a>如何使用

Reveal 能自動搭配一些控制項運作。 針對其他控制項，您可以藉由指派特殊樣式給該控制項來啟用顯示，如本文的[在其他控制項上啟用 Reveal](#enabling-reveal-on-other-controls) 和[在自訂控制項上啟用 Reveal](#enabling-reveal-on-custom-controls) 小節中所述。

## <a name="controls-that-automatically-use-reveal"></a>會自動使用 Reveal 的控制項

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

這些圖例顯示數個不同控制項上的顯示顯目提示：

![Reveal 範例](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>在其他控制項上啟用 Reveal

如果您有應套用 Reveal 的案例 (這些控制項是主要內容和/或被用於清單或集合方向中)，我們提供選擇加入資源樣式，讓您能夠針對那類狀況啟用 Reveal。

這些控制項預設沒有 Reveal，因為它們是較小的控制項，且通常是您應用程式主要焦點的協助程式控制項；但每個應用程式都不相同，因此如果在您的應用程式中最常使用這些控制項，我們提供一些樣式來加以協助：

| 控制項名稱   | 資源名稱 |
|----------|:-------------:|
| 按鈕 |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem (內容之上的 Reveal) | GridViewItemRevealBackgroundShowsAboveContentStyle |

若要套用這些樣式，只需設定控制項的 [Style](/uwp/api/Windows.UI.Xaml.Style) 屬性：

```xaml
<Button Content="Button Content" Style="{ThemeResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>佈景主題中的 Reveal

Reveal 會根據控制項、應用程式或使用者設定所要求的佈景主題而稍微變更。 在深色佈景主題中，Reveal 的框線及暫留的光線為白色，但在淺色佈景主題中，只有框線會暗化為淺灰色。

![深色與淺色 Reveal](images/Dark_vs_LightReveal.png)

若要在淺色佈景主題中啟用白色框線，只需將控制項上所要求的佈景主題設定為 Dark。

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

或是將 RevealBorderBrush 上的 TargetTheme 變更為 Dark。 請記住！ 如果 TargetTheme 已設定為 Dark，則 Reveal 將會是白色；但如果它是設定為 Light，則 Reveal 的框線將會是灰色。

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>在自訂控制項上啟用 Reveal

您可以將 Reveal 新增至自訂控制項。 在執行此動作之前，先多了解一點 Reveal 效果的運作方式，將會很有幫助。 Reveal 是由兩個不同的效果所組成：**顯示框線**和**顯示暫留**。

- **框線**會在指標靠近互動式元素時，顯示其框線。 這個效果會告訴您那些附近的物件可以採取類似於目前焦點所在之物件的動作。
- **暫留**會在暫留或焦點所在項目的周圍套用柔和的光暈形狀，並在按一下時播放按下動畫。 

![Reveal 層](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


這些效果是由兩個筆刷所定義： 
* 框線顯示由 **RevealBorderBrush** 所定義
* 暫留顯示由 **RevealBackgroundBrush** 所定義

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
在大部分情況下，我們會針對特定控制項自動開啟 Reveal，來處理這兩個筆刷的使用。 不過，其他控制項必須透過套用樣式或直接變更其範本來啟用。

### <a name="when-to-add-reveal"></a>加入 Reveal 的時機
您可以將 Reveal 加入自訂控制項，但在執行此動作之前，請先考慮控制項的類型及其運作方式。 
* 如果自訂控制項是單一互動式元素，且沒有相似的控制項共用其空間 (例如功能表中的功能表項目)，您的自訂控制項可能就不需要 Reveal。  
* 如果您有相關的互動式內容或元素的群組，則應用程式的該區域便很可能需要 Reveal；這通常被稱為[命令](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding) \(部分機器翻譯\) 介面。

例如，按鈕本身不應使用 Reveal，但是命令列中的一組按鈕就應該使用 Reveal。

<!-- For example, NavigationView's items are related to page navigation. CommandBar's buttons relate to menu actions or page feature actions. MediaTransportControl's buttons beneath all relate to the media being played. -->

### <a name="using-the-control-template-to-add-reveal"></a>使用控制項範本來加入 Reveal 
若要在自訂控制項或重新範本化的控制項上啟用 Reveal，您可以修改控制項的控制範本。 大多數控制項範本的根目錄都有格線；請更新該根格線的 [VisualState](/uwp/api/windows.ui.xaml.visualstate) 以使用 Reveal：

```xaml
<VisualState x:Name="PointerOver">
    <VisualState.Setters>
        <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
        <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
        <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
        <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
    </VisualState.Setters>
</VisualState>
```

請務必注意，Reveal 在其視覺狀態中必須同時具有筆刷和 setter 才能正常運作。 單純將控制項的筆刷設定為其中一個 Reveal 筆刷資源，並無法針對該控制項啟用 Reveal。 相反地，只有目標或設定，而沒有 Reveal 筆刷的值，也無法啟用 Reveal。

若要深入了解修改控制項範本，請參閱 [XAML 控制項範本](../controls-and-patterns/control-templates.md)文章。

我們已建立一組系統 Reveal 筆刷，可供您用來自訂範本。 例如，您可以使用 **ButtonRevealBackground** 筆刷來建立自訂按鈕背景，或使用 **ListViewItemRevealBackground** 筆刷來建立自訂清單，依此類推。 (若要了解資源在 XAML 中的運作方式，請參閱 [Xaml 資源字典](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)文章。)

### <a name="full-template-example"></a>完整範本範例

以下是能呈現 Reveal 按鈕外觀的完整範本：

```xaml
<Style TargetType="Button" x:Key="ButtonStyle1">
    <Setter Property="Background" Value="{ThemeResource ButtonRevealBackground}" />
    <Setter Property="Foreground" Value="{ThemeResource ButtonForeground}" />
    <Setter Property="BorderBrush" Value="{ThemeResource ButtonRevealBorderBrush}" />
    <Setter Property="BorderThickness" Value="2" />
    <Setter Property="Padding" Value="8,4,8,4" />
    <Setter Property="HorizontalAlignment" Value="Left" />
    <Setter Property="VerticalAlignment" Value="Center" />
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}" />
    <Setter Property="FontWeight" Value="Normal" />
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="UseSystemFocusVisuals" Value="True" />
    <Setter Property="FocusVisualMargin" Value="-3" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid x:Name="RootGrid" Background="{TemplateBinding Background}">

                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="PointerOver">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Pressed">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="Pressed" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerDownThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundDisabled}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushDisabled}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundDisabled}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>

              </VisualStateManager.VisualStateGroups>
                    <ContentPresenter x:Name="ContentPresenter"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}"
                    Content="{TemplateBinding Content}"
                    ContentTransitions="{TemplateBinding ContentTransitions}"
                    ContentTemplate="{TemplateBinding ContentTemplate}"
                    Padding="{TemplateBinding Padding}"
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
                    AutomationProperties.AccessibilityView="Raw" />
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

### <a name="fine-tuning-the-reveal-effect-on-a-custom-control"></a>微調自訂控制項上的 Reveal 效果 

在自訂或重新樣本化的控制項，或是自訂命令表面上啟用 Reveal 時，這些提示可以協助您將效果最佳化：
 
* 在高度或寬度不一致的相鄰項目上 (特別是在清單中)：移除框線方法行為，並讓框線只會在暫留時顯示。
* 針對經常會切換停用狀態的命令項目：將框線方法筆刷置於元素的背板及框線上來強調其狀態。
* 針對距離無比接近而互相碰觸的相鄰命令元素：在兩個元素之間加入 1px 的邊界。 

## <a name="dos-and-donts"></a>可行與禁止事項
### <a name="do"></a>可行事項：
- 在使用者可以採取許多動作的元素 (命令列、瀏覽功能表) 上使用 Reveal
- 在預設沒有視覺分隔項目的互動式元素群組 (清單、功能區) 中使用 Reveal
- 在具有高度密集互動式元素的區域 (命令案例) 中使用 Reveal
- 在 Reveal 項目之間放置 1px 的邊界空間

### <a name="dont"></a>禁止事項
- 不要在靜態內容 (背景、文字) 上使用 Reveal
- 不要在快顯視窗、飛出視窗或下拉式清單上使用 Reveal
- 不要在一次性的獨立情況中使用 Reveal
- 不要在非常大的項目 (大於 500epx) 上使用 Reveal
- 不要在安全性決策中使用 Reveal，因為它可能會轉移使用者對您必須提供給他們之訊息的注意力


## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="reveal-and-the-fluent-design-system"></a>Reveal 和 Fluent Design 系統

 Fluent Design 系統能協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 Reveal 是能將光源加入應用程式中的 Fluent Design 系統元件。 若要深入了解，請參閱[適用於 UWP 的 Fluent Design 概觀](/windows/apps/fluent-design-system)。

## <a name="related-articles"></a>相關文章

- [RevealBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush) \(英文\)
- [壓克力](acrylic.md)
- [組合效果](https://docs.microsoft.com/windows/uwp/graphics/composition-effects) \(部分機器翻譯\)
- [適用於 UWP 的 Fluent Design](/windows/apps/fluent-design-system)
- [系統中的科學：Fluent Design 和深度](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f) \(英文\)
- [系統中的科學：Fluent Design 和光線](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f) \(英文\)
