---
author: mijacobs
description: "顯色是一種光源效果，有助於讓應用程式的互動元素更具深度感並吸引注意力。"
title: "顯色顯目提示"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: high
ms.openlocfilehash: 8ba0d9939d7ab1d9826ed2848e476499f09c628f
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2018
---
# <a name="reveal-highlight"></a>顯色顯目提示

顯色是一種光源效果，有助於讓應用程式的互動元素更具深度感並吸引注意力。

> **重要的 API**：[RevealBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)、[RevealBackgroundBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush)、[RevealBorderBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush)、[RevealBrushHelper 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper)、[VisualState 類別](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

顯色行為透過當指標在附近時顯示可點選內容的容器來達到這個目的。

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
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="reveal-and-the-fluent-design-system"></a>顯色和 Fluent 設計系統

 Fluent 設計系統協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 顯色是將光源加入應用程式中的 Fluent 設計系統元件。 若要深入瞭解，請參閱[適用於 UWP 的 Fluent 設計概觀](../fluent-design-system/index.md)。

## <a name="how-to-use-it"></a>如何使用

顯色會在一些控制項上自動運作。 至於其他控制項，您可以指派特殊樣式給控制項來啟用顯色。

## <a name="controls-that-automatically-use-reveal"></a>自動使用顯色的控制項

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)
- [**ComboBox**](../controls-and-patterns/lists.md)

下圖顯示數個不同控制項上的顯色效果：

![顯色範例](images/RevealExamples_Collage.png)

## <a name="enabling-reveal-on-other-common-controls"></a>在其他常見控制項上啟用顯色

如果您有一個應該套用顯色的案例 (這些控制項為主要內容和/或用於清單或集合方向中)，我們提供了選擇加入資源樣式，讓您能夠針對這幾類的狀況啟用顯色。

這些控制項預設沒有顯色，因為它們是較小的控制項，且通常是您應用程式主要焦點的協助程式控制項；但每個應用程式都不相同，如果在您的應用程式中最常使用這些控制項，我們已提供一些樣式加以協助：

| 控制項名稱   | 資源名稱 |
|----------|:-------------:|
| Button |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |

若要套用這些樣式，只需更新 Style 屬性，如下所示：

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

## <a name="enabling-reveal-on-custom-controls"></a>在自訂控制項上啟用顯色

在決定自訂控制項是否應獲得顯色時應思考的通則是，您必須有一組互動元素，這些元素都與您希望在應用程式中執行的核心功能或動作有所關聯。

例如，NavigationView 的項目與網頁瀏覽有關。 CommandBar 的按鈕與功能表動作或頁面功能動作有關。 下方的 MediaTransportControl 按鈕都與要播放的媒體有關。

取得顯色的控制項不需要彼此相關，只需位在高密度區域中並用於更大的用途。

若要在自訂控項或重新樣板化控制項上啟用顯色，您可以在該控制項範本的 \[視覺狀態\] 中移至該控制項的樣式，然後在根格線上指定顯色：

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

請務必注意，顯色的視覺狀態中必須同時具備筆刷和 setter 才能完整運作。 單純只將控制項的筆刷設定為我們的其中一個顯色筆刷資源，並無法啟用該控制項的顯色。 反過來，只有目標或設定但值沒有顯色筆刷，也無法啟用顯色。

我們已建立一組系統顯色筆刷，供您用來自訂您的 UI。 例如，您可以使用 **ButtonRevealBackground** 筆刷來建立自訂按鈕背景，或使用 **ListViewItemRevealBackground** 筆刷來建立自訂清單，以此類推。

(若要深入了解 XAMl 中資源的運作方式，請參閱 [Xaml 資源字典](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)文章。)

### <a name="reveal-on-listview-controls-with-nested-buttons"></a>包含巢狀按鈕的 ListView 控制項上的顯色

如果您有 [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)，且在其 [ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem) 元素中有巢狀的按鈕或可叫用內容，您應該為巢狀項目啟用顯色。

如果是按鈕或 ListViewItem 中類似按鈕的控制項，只需將控制項的 [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_Style) 屬性設定為 **ButtonRevealStyle** 靜態資源。 

![巢狀顯色](images/NestedListContent.png)

此範例將在 ListViewItem 中的許多按鈕上啟用顯色。 

```XAML
<ListViewItem>
    <StackPanel Orientation="Horizontal">
        <TextBlock Margin="5">Test Text: lorem ipsum.</TextBlock>
        <StackPanel Orientation="Horizontal">
            <Button Content="&#xE71B;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE728;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE74D;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
         </StackPanel>
    </StackPanel>
</ListViewItem>
```

### <a name="listviewitempresenter-with-reveal"></a>具有顯色的 ListViewItemPresenter

清單在 XAML 中有點特殊，對於顯色案例，我們必須在 ListViewItemPresenter 中只為顯色定義 Visual State Manager：

```XAML
<ListViewItemPresenter>
<!-- ContentTransitions, SelectedForeground, etc. properties -->
RevealBackground="{ThemeResource ListViewItemRevealBackground}"
RevealBorderThickness="{ThemeResource ListViewItemRevealBorderThemeThickness}"
RevealBorderBrush="{ThemeResource ListViewItemRevealBorderBrush}">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
        <VisualState x:Name="Normal" />
        <VisualState x:Name="Selected" />
        <VisualState x:Name="PointerOver">
            <VisualState.Setters>
                <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
            </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverPressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PressedSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            </VisualStateGroup>
                <VisualStateGroup x:Name="EnabledGroup">
                    <VisualState x:Name="Enabled" />
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                             <Setter Target="Root.RevealBorderThickness" Value="0"/>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ListViewItemPresenter>
```

這表示在 ListViewItemPresenter 中使用顯色特定 Visual State setter 附加至屬性集合結尾。

### <a name="full-template-example"></a>完整範本範例

以下是顯色按鈕的整個範本：

```XAML
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

## <a name="dos-and-donts"></a>可行與禁止注意事項
- 務必在使用者可採取動作 (按鈕、選取項目) 的元素上使用顯色
- 務必在預設沒有視覺分隔符號的互動元素群組 (清單、命令列) 中使用顯色
- 務必在具有高密度互動元素的區域中使用顯色
- 不要在靜態內容 (背景、文字) 使用顯色
- 不要在一次性的隔離情形中使用顯色
- 不要在非常大的項目 (大於 500epx) 上使用顯色
- 不要在安全決策中使用顯色，因為可能會轉移對您必須提供給使用者之訊息的注意力

## <a name="how-we-designed-reveal"></a>我們如何設計顯色

顯色有兩個主要的視覺元件：**暫留顯色**行為與**邊框顯色**行為。

![顯色層級](images/RevealLayers.png)

暫留顯色直接與正在暫留 (透過指標或焦點輸入) 的內容繫結，並會在暫留或聚焦的項目周圍套用柔和的光暈形狀，讓您知道您可以與該項目互動。

邊框顯色會套用到焦點項目與附近的項目。 這會讓您看到這些附近物件會採取與目前所聚焦物件類似的動作。

顯色配方明細為：

- 邊框顯色將會在所有內容的頂端，但在指定的邊緣上
- 文字與內容將直接顯示在邊框顯色下方
- 暫留顯色將位在內容與文字下方
- 背板 (開啟與啟用暫留顯色)
- 背景 (控制項背景)

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->  

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [RevealBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [壓克力](acrylic.md)
- [組合效果](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [適用於 UWP 的 Fluent 設計](../fluent-design-system/index.md)
- [系統中的科學：Fluent 設計和深度](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [系統中的科學：Fluent 設計和光源](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
