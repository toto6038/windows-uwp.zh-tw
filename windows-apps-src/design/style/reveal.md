---
description: 顯色是一種光源效果，有助於讓應用程式的互動元素更具深度感並吸引注意力。
title: 顯示反白顯示
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5cb076de6cd9c44280bf7030a59c645f601487bd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370436"
---
# <a name="reveal-highlight"></a>顯示反白顯示

![主角圖像](images/header-reveal-highlight.svg)

顯示反白顯示是互動式的項目，例如命令列，反白顯示，當使用者將滑鼠指標靠近其光源效果。 

> **重要的 Api**:[RevealBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)， [RevealBackgroundBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush)， [RevealBorderBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush)， [RevealBrushHelper 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper)， [VisualState類別](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

## <a name="how-it-works"></a>運作方式
顯示反白顯示呼叫注意給互動項目所顯示的項目容器，當指標位於附近下, 圖所示：

![顯色視覺效果](images/Nav_Reveal_Animation.gif)

透過公開物件四周的隱藏邊框，顯色讓使用者能更進一步了解他們正在互動的空間，並幫助他們了解可用的動作。 這對清單控制項以及按鈕群組尤其重要。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下此處以<a href="xamlcontrolsgallery:/item/Reveal">開啟應用程式並查看顯色效果運作情形</a>。</p>
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

顯色會自動在一些控制項上運作。 對於其他控制項，您可以啟用顯示指派特殊的樣式至控制項，如中所述[啟用顯示其他控制項](#enabling-reveal-on-other-controls)並[啟用自訂控制項上顯示](#enabling-reveal-on-custom-controls)這個區段發行項。

## <a name="controls-that-automatically-use-reveal"></a>自動使用顯色的控制項

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

這些圖例顯示數個不同的控制項顯示反白顯示：

![顯色範例](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>在其他控制項上啟用顯色

如果您有一個應該套用顯色的案例 (這些控制項為主要內容和/或用於清單或集合方向中)，我們提供了選擇加入資源樣式，讓您能夠針對這幾類的狀況啟用顯色。

這些控制項預設沒有顯色，因為它們是較小的控制項，且通常是您應用程式主要焦點的協助程式控制項；但每個應用程式都不相同，如果在您的應用程式中最常使用這些控制項，我們已提供一些樣式加以協助：

| 控制項名稱   | Resource Name |
|----------|:-------------:|
| 按鈕 |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem (內容之上的顯色) | GridViewItemRevealBackgroundShowsAboveContentStyle |

若要套用這些樣式，只需設定控制項的 [Style](/uwp/api/Windows.UI.Xaml.Style) 屬性：

```xaml
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>佈景主題中的顯色

顯色會根據控制項、應用程式或使用者設定要求的佈景主題稍微變更。 在深色佈景主題顯色的框線及暫留中，淺色為白色，但在淺色佈景主題中則是框線暗化為淺灰色。

![深色與淺色顯色](images/Dark_vs_LightReveal.png)

若要在佈景主題為淺色時啟用白色框線，只需將控制項上要求的佈景主題設定為 [深色]。

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

或是將 RevealBorderBrush 上的 TargetTheme 變更為 [深色]。 記住！ 如果 TargetTheme 已設定為 [深色]，則顯色為白色，但如果是設定為 [淺色]，則顯色的框線會是灰色。

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>在自訂控制項上啟用顯色

您可以將顯色新增至自訂控制項。 這麼做之前，最好稍微了解的顯示效果的運作方式。 顯示組成兩個不同的效果：**顯示框線**並**顯示暫留**。

- **\[框線\]** 會在指標靠近時顯示互動式元素的框線。 這個效果會告訴您這些附近的物件可以採取類似於目前焦點所在物件的動作。
- **\[暫留\]** 會在暫留或焦點所在項目周圍套用柔和的光暈形狀，並在按一下時播放按下動畫。 

![顯色圖層](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


這些效果是由兩個筆刷定義： 
* 框線顯示由定義**RevealBorderBrush**
* 暫留時顯示由定義**RevealBackgroundBrush**

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
在大部分情況下，我們會讓特定控制項的顯色自動開啟，以便處理這兩個筆刷的使用。 不過，其他控制項則必須透過套用樣式或直接變更其範本，才能啟用顯色。

### <a name="when-to-add-reveal"></a>新增顯色的時機
您可以將顯色新增到自訂控制項，但在執行此動作之前，請先考慮控制項類型及其運作方式。 
* 如果自訂控制項是單一互動式元素，且沒有相似的控制項共用其空間 (例如功能表中的功能表項目)，您自訂控制項可能就不需要顯色。  
* 如果您有相關互動式內容或元素的群組，則 App 的該區域確實需要顯色，這通常稱為[命令](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding)介面。

例如，按鈕本身不應使用顯色，但是命令列中的一組按鈕設定就需要使用顯色。

<!-- For example, NavigationView's items are related to page navigation. CommandBar's buttons relate to menu actions or page feature actions. MediaTransportControl's buttons beneath all relate to the media being played. -->

### <a name="using-the-control-template-to-add-reveal"></a>使用控制項範本來新增顯色 
若要在自訂控制項或重新樣板化控制項上啟用顯色，您可以修改控制項的範本。 大多數控制項範本在根層級都有格線；更新此根格線的 [VisualState](/uwp/api/windows.ui.xaml.visualstate) 即可使用顯色。

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

請務必注意，顯色在其視覺狀態中必須筆刷和 setter 兼備，才能正常運作。 只是單純地將控制項的筆刷設定為其中一個顯色筆刷資源，並不能啟用該控制項的顯色。 相反地，徒有其值不為顯色筆刷的目標或設定，同樣無法啟用顯色。

若要深入了解修改控制項範本，請參閱 [XAML 控制項範本](../controls-and-patterns/control-templates.md)文章。

我們已建立一組系統顯色筆刷，您可以用來自訂您的範本。 例如，您可以使用 **ButtonRevealBackground** 筆刷來建立自訂按鈕背景，或使用 **ListViewItemRevealBackground** 筆刷來建立自訂清單，以此類推。 (若要了解資源在 XAML 中的運作方式，請參閱 [Xaml 資源字典](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)文章)。

### <a name="full-template-example"></a>完整範本範例

以下是呈現顯色按鈕外觀的完整範本：

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

### <a name="fine-tuning-the-reveal-effect-on-a-custom-control"></a>微調自訂控制項上的顯色效果 

當您啟用顯示的自訂或 re 樣板化控制項或自訂的命令介面上時，這些提示可協助您最佳化效果：
 
* 在相鄰的項目大小的不一致的高度或寬度 （特別是在清單中）：移除框線方法行為，並保留已顯示只有暫留時的框線。
* 命令的項目，經常移入和登出停用狀態的：地方上的項目 backplates，以及其框線的框線方法筆刷來強調其狀態。
* 針對相鄰命令項目非常接近其碰觸到：加入 1px 邊界之間的兩個元素。 

## <a name="dos-and-donts"></a>可行與禁止事項
### <a name="do"></a>執行動作：
- 要在使用者可以執行許多動作的元素 (命令列、瀏覽功能表) 上使用顯色
- 要在預設沒有視覺分隔線的互動式元素群組 (清單、功能區) 中使用顯色
- 要在互動式元素密集度高的區域 (命令功能案例) 中使用顯色
- 要在顯色項目之間放置 1px 邊界空間

### <a name="dont"></a>禁止事項
- 不要在靜態內容 (背景、文字) 上使用顯色
- 不要在快顯視窗、飛出視窗或下拉式清單上使用顯色
- 不要在僅出現一次的單獨情況中使用顯色
- 不要在非常大的項目 (大於 500epx) 上使用顯色
- 不要在安全決策中使用顯色，因為可能會轉移對您必須提供給使用者之訊息的注意力


## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以互動式格式查看所有 XAML 控制項。

## <a name="reveal-and-the-fluent-design-system"></a>顯色和 Fluent Design 系統

 Fluent Design 系統協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 顯色是將光源加入應用程式中的 Fluent Design 系統元件。 若要深入了解，請參閱[適用於 UWP 的 Fluent Design 概觀](/windows/apps/fluent-design-system)。

## <a name="related-articles"></a>相關文章

- [RevealBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrylic](acrylic.md)
- [撰寫效果](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [適用於 UWP 的 Fluent 設計](/windows/apps/fluent-design-system)
- [在系統中的科學：Fluent 設計和深度](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [在系統中的科學：Fluent Design and 光線](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
