---
Description: TwoPaneView 是一種版面配置控制項，可協助您管理具有 2 個不同內容區域的應用程式顯示。
title: 兩個窗格檢視
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e151e06f0ebc838671aa1100d96e8e6f14de0739
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "80614129"
---
# <a name="two-pane-view"></a>兩個窗格檢視

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) 是一種版面配置控制項，可協助您管理具有 2 個不同內容區域的應用程式顯示，例如主要/詳細資料檢視。

> [!IMPORTANT]
> 本文說明處於公開預覽狀態的功能和指導方針，在正式推出之前可能會大幅修改。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

雖然它適用於所有 Windows 裝置，但是 TwoPaneView 控制項的設計目的是協助您自動充分利用雙螢幕裝置，而不需要特殊的編碼。 在雙螢幕裝置上，兩個窗格檢視可確保使用者介面 (UI) 在跨越螢幕之間的間隙時清楚地分割，如此一來，您的內容就會顯示在間距的任一側。

> [!NOTE]
> _雙螢幕裝置_是一種特殊裝置，具有獨特的功能。 這不等同於具有多個監視器的桌面裝置。 如需雙螢幕裝置的詳細資訊，請參閱[雙螢幕裝置簡介](/dual-screen/introduction)。 (如需您可以針對多個監視器最佳化應用程式的方式詳細資訊，請參閱[顯示多重檢視](/windows/uwp/design/layout/show-multiple-views)。)

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | **TwoPaneView** 控制項包含在 Windows UI 程式庫中；此程式庫是包含適用於 UWP 應用程式的新控制項和 UI 功能的 NuGet 套件。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **Windows UI 程式庫 API：** [TwoPaneView 類別](/uwp/api/microsoft.ui.xaml.controls.twopaneview)

> [!TIP]
> 在這整份文件中，我們使用 XAML 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已將此新增至我們的 [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在後方的程式碼中，我們也使用 C# 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已在檔案頂端新增了此 **using** 陳述式：`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

當您有 2 個不同的內容區域時，使用兩個窗格檢視：

- 內容應該會自動重新排列並調整大小，以最適合視窗。
- 內容的次要區域應該根據可用空間顯示/隱藏。
- 內容應該在雙螢幕裝置的 2 個畫面之間清楚地分割。

## <a name="examples"></a>範例

這些影像會顯示在單一螢幕上和跨雙螢幕執行的應用程式。 雙窗格的檢視會調整應用程式 UI 以符合各種螢幕設定。

![單一螢幕上的雙窗格檢視應用程式](images/two-pane-view/tpv-single.png)

> _單一螢幕上的應用程式。_

![雙螢幕上的雙窗格檢視應用程式 (寬模式)](images/two-pane-view/tpv-dual-wide.png)

> _跨雙螢幕裝置的應用程式 (寬模式)。_

![雙螢幕上的雙窗格檢視應用程式 (高模式)](images/two-pane-view/tpv-dual-tall.png)

> _跨雙螢幕裝置的應用程式 (高模式)。_

## <a name="how-it-works"></a>運作方式

兩個窗格檢視有兩個窗格，您可以在其中放置內容。 會根據視窗的可用空間來調整窗格的大小和排列。 可能的窗格版面配置是由 [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode) 列舉所定義：

| 列舉&nbsp;值 | 說明 |
| - | - |
| `SinglePane` | 只會顯示一個窗格，如 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 屬性所指定。 |
| `Wide` | 並排顯示窗格，或顯示單一窗格，如 [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) 屬性所指定。 |
| `Tall` | 上下顯示窗格，或顯示單一窗格，如 [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) 屬性所指定。 |

您可以藉由設定 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 以指定當只有一個窗格的空間時，要顯示哪一個窗格，以設定兩個窗格檢視。 然後，您可以指定 `Pane1` 顯示在高視窗的上方或下方，或是在寬視窗的左側或右側。

兩個窗格檢視會處理窗格的大小和排列，但是您仍然需要因應大小和方向的變更來調適窗格內的內容。 如需有關建立調適型 UI 的詳細資訊，請參閱[搭配 XAML 的回應式版面配置](/windows/uwp/design/layout/layouts-with-xaml)和[版面配置面板](/windows/uwp/design/layout/layout-panels)。

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) 會根據應用程式的跨越狀態來管理窗格的顯示。

- 在單一螢幕上

    當您的應用程式僅顯示於單一螢幕時，`TwoPaneView` 會根據您指定的屬性設定來調整其窗格的大小和位置。 在下一節中，我們會更詳細地說明這些屬性。 裝置之間唯一的差異在於，某些裝置 (例如桌上型電腦) 允許可調整大小的視窗，而其他裝置則否。

- 跨雙螢幕

    `TwoPaneView` 的設計可讓您輕鬆地最佳化跨雙螢幕裝置的 UI。 視窗會自行調整大小，以使用螢幕上的所有可用空間。 當您的應用程式跨越雙螢幕裝置的兩個螢幕時，每個螢幕會顯示其中一個窗格的內容，並且讓內容適當地跨越間距。 當您使用兩個窗格檢視時，會內建跨越感知功能。 您只需要設定高/寬設定，以指定要在哪一個螢幕上顯示哪一個窗格。 兩個窗格檢視會負責處理其餘部分。

## <a name="how-to-use-the-two-pane-view-control"></a>如何使用兩個窗格檢視控制項

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) 不一定要是您頁面版面配置的根元素。 事實上，您通常會在提供應用程式整體導覽的 [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) 控制項內使用它。 `TwoPaneView` 無論位於 XAML 樹狀結構中的何處都會適當調整；但建議您不要將 `TwoPaneView` 內嵌在另一個 `TwoPaneView` 中。 (若這麼做，將只有外部 `TwoPaneView` 可感知跨越)。

### <a name="add-content-to-the-panes"></a>將內容新增至窗格

雙窗格檢視的每個窗格分別可包含單一 XAML `UIElement`。 若要新增內容，您通常會在每個窗格中放置 XAML 版面配置面板，然後將其他控制項和內容新增至面板。 窗格可以變更大小，並且在寬和高模式之間切換，因此您必須確定每個窗格中的內容都可以適應這些變更。 如需有關建立調適型 UI 的詳細資訊，請參閱[搭配 XAML 的回應式版面配置](/windows/uwp/design/layout/layouts-with-xaml)和[版面配置面板](/windows/uwp/design/layout/layout-panels)。

此範例會建立先前顯示於_範例_一節中的簡單圖片/資訊應用程式 UI。 當應用程式跨雙螢幕時，圖片和資訊會顯示在不同的螢幕上。 在單一螢幕上，內容可顯示在兩個窗格中，或結合成單一窗格，視可用的空間量而定。 (當只有一個窗格的空間時，您會將 Pane2 的內容移至 Pane1，並且讓使用者捲動以查看任何隱藏的內容。 您稍後會在_回應模式變更_一節中看到這個部分的程式碼。)

![跨雙螢幕的範例應用程式的小影像](images/two-pane-view/tpv-left-right.png)

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" MinHeight="40"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <CommandBar DefaultLabelPosition="Right">
        <AppBarButton x:Name="Share" Icon="Share" Label="Share" Click="Share_Click"/>
        <AppBarButton x:Name="Print" Icon="Print" Label="Print" Click="Print_Click"/>
    </CommandBar>

    <muxc:TwoPaneView
        x:Name="MyTwoPaneView"
        Grid.Row="1"
        MinWideModeWidth="959"
        MinTallModeHeight="863"
        ModeChanged="TwoPaneView_ModeChanged">

        <muxc:TwoPaneView.Pane1>
            <Grid x:Name="Pane1Root">
                <ScrollViewer>
                    <StackPanel x:Name="Pane1StackPanel">
                        <Image Source="Assets\LandscapeImage8.jpg"
                               VerticalAlignment="Top" HorizontalAlignment="Center"
                               Margin="16,0"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane1>

        <muxc:TwoPaneView.Pane2>
            <Grid x:Name="Pane2Root">
                <ScrollViewer x:Name="DetailsContent">
                    <StackPanel Padding="16">
                        <TextBlock Text="Mountain.jpg" MaxLines="1"
                                       Style="{ThemeResource HeaderTextBlockStyle}"/>
                        <TextBlock Text="Date Taken:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="8/29/2019 9:55am"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Dimensions:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="800x536"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Resolution:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="96 dpi"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Description:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Maecenas porttitor congue massa. Fusce posuere, magna sed pulvinar ultricies, purus lectus malesuada libero, sit amet commodo magna eros quis urna."
                                       Style="{ThemeResource SubtitleTextBlockStyle}"
                                       TextWrapping="Wrap"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane2>
    </muxc:TwoPaneView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="TwoPaneViewStates">
            <VisualState x:Name="Normal"/>
            <VisualState x:Name="Wide">
                <VisualState.Setters>
                    <Setter Target="MyTwoPaneView.Pane1Length"
                            Value="2*"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="specify-which-pane-to-display"></a>指定要顯示的窗格

當兩個窗格檢視只能顯示單一窗格時，它會使用 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 屬性來決定要顯示哪一個窗格。 根據預設，PanePriority 會設定為 **Pane1**。 以下是您可以在 XAML 或程式碼中設定此屬性的方式。

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" PanePriority="Pane2">
```

```csharp
MyTwoPaneView.PanePriority = Microsoft.UI.Xaml.Controls.TwoPaneViewPriority.Pane2;
```

### <a name="pane-sizing"></a>調整窗格大小

在單一螢幕上，窗格的大小取決於 [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) 和 [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length) 屬性。 這些屬性會使用 [GridLength](/uwp/api/windows.ui.xaml.gridlength) 值，支援_自動_和_星號_(\*)調整大小。 如需自動和以星號調整大小的說明，請參閱[搭配 XAML 的回應式版面配置](/windows/uwp/design/layout/layouts-with-xaml#layout-properties)的_版面配置屬性_一節。

根據預設，`Pane1Length` 會設定為 `Auto`，而且會自行調整大小以符合其內容。 `Pane2Length` 會設定為 `*`，且會使用所有剩餘的空間。

![將窗格設定為預設大小的雙窗格檢視](images/two-pane-view/tpv-size-default.png)

> _具有預設大小的窗格_

預設值適用於一般主要/詳細資料版面配置，其中，您可在 `Pane1` 中取得項目清單，在 `Pane2` 中取得許多詳細資料。 不過，視您的內容而定，您可能會想要以不同的方式分割空間。 在這裡，`Pane1Length` 設定為 `2*`，因此所取得的空間會是 `Pane2` 的兩倍。

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![窗格 1 使用了三分之二的螢幕、窗格 2 使用三分之一螢幕的雙窗格檢視](images/two-pane-view/tpv-size-2.png)

> _窗格大小 2* 和 *_

> [!NOTE]
> 如前所述，當應用程式跨雙螢幕時，將會忽略這些屬性，讓各個窗格填滿其中一個螢幕。

如果您將窗格設定為使用自動調整大小，您可以藉由設定保留窗格內容的面板高度和寬度，來控制大小。 在此情況下，您可能需要處理 ModeChanged 事件，並將內容的高度和寬度條件約束設定為適用於目前的模式。

### <a name="display-in-wide-or-tall-mode"></a>以寬或高模式顯示

在單一螢幕上，雙窗格檢視的顯示[模式](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode)取決於 [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) 和 [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight) 屬性。 這兩個屬性的預設值都是641px，與 [NavigationView. CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) 相同。

下表顯示 TwoPaneView 的「高度」和「寬度」如何決定所使用的顯示模式。

| TwoPaneView 條件  | 模式 |
|---------|---------|
| `Width` > `MinWideModeWidth` | 使用 `Wide` 模式 |
| `Width` <= `MinWideModeWidth` 和 `Height` > `MinTallModeHeight` | 使用 `Tall` 模式 |
| `Width` <= `MinWideModeWidth` 和 `Height` <= `MinTallModeHeight` | 使用 `SinglePane` 模式 |


> [!NOTE]
> 如前所述，當應用程式跨雙螢幕時，將會忽略這些屬性，而根據裝置_狀態_來決定顯示模式。

#### <a name="wide-configuration-options"></a>寬設定選項

當單一顯示器寬於 `MinWideModeWidth` 屬性時，雙窗格檢視就會進入 `Wide` 模式。 `MinWideModeWidth` 會控制雙窗格檢視進入寬模式的時機。 預設值為 641px，但是您可以將它變更為您想要的任何值。 一般來說，您應該將此屬性設定為您希望窗格成為最小寬度的任何值。

當雙窗格檢視處於寬模式時，[WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) 屬性會決定所要顯示的內容：

| [列舉&nbsp;值](/uwp/api/microsoft.ui.xaml.controls.twopaneviewwidemodeconfiguration) | 說明 |
|---------|---------|
| `SinglePane` | 單一窗格 (由 `PanePriority` 決定)。 此窗格會佔用 `TwoPaneView` 的完整大小 (亦即，兩個方向都是以星號調整大小)。 |
| `LeftRight` | `Pane1` 在左側/`Pane2` 在右側。 兩個窗格都是以垂直方向以星號調整大小，`Pane1` 的寬度是自動調整，而 `Pane2` 的寬度則是以星號調整大小。 |
| `RightLeft` | `Pane1` 在右側/`Pane2` 在左側。 兩個窗格都是以垂直方向以星號調整大小，`Pane2` 的寬度是自動調整，而 `Pane1` 的寬度則是以星號調整大小。 |

預設設定是 `LeftRight`。

| LeftRight | RightLeft |
| - | - |
| ![設定為由左至右的雙窗格檢視](images/two-pane-view/tpv-left-right.png)  | ![設定為由右至左的雙窗格檢視](images/two-pane-view/tpv-right-left.png)  |

> **提示：** 當裝置使用由右至左 (RTL) 語言時，兩個窗格檢視會自動交換順序：`RightLeft` 會顯示為 `LeftRight`，`LeftRight` 則顯示為 `RightLeft`。

#### <a name="tall-configuration-options"></a>高設定選項

當單一顯示器窄於 `MinWideModeWidth` 且高於 `MinTallModeHeight` 時，雙窗格檢視就會進入 `Tall` 模式。 預設值為 641px，但是您可以將它變更為您想要的任何值。 一般來說，您應該將此屬性設定為您希望窗格成為最小高度的任何值。

當雙窗格檢視處於高模式時，[TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) 屬性會決定所要顯示的內容：

| [列舉&nbsp;值](/uwp/api/microsoft.ui.xaml.controls.twopaneviewtallmodeconfiguration) | 說明 |
|---------|---------|
| `SinglePane` | 單一窗格 (由 `PanePriority` 決定)。 此窗格會佔用 `TwoPaneView` 的完整大小 (亦即，兩個方向都是以星號調整大小)。 |
| `TopBottom` | `Pane1` 位於頂端/`Pane2` 位於底部。 兩個窗格都是以水平方向以星號調整大小，`Pane1` 的高度是自動調整，而 `Pane2` 的高度則是以星號調整大小。 |
| `BottomTop` | `Pane1` 位於底部/`Pane2` 位於頂端。 兩個窗格都是以水平方向以星號調整大小，`Pane2` 的高度是自動調整，而 `Pane1` 的高度則是以星號調整大小。 |

預設值為 `TopBottom`。

| TopBottom | BottomTop |
| - | - |
| ![設定為由上至下的雙窗格檢視](images/two-pane-view/tpv-top-bottom.png)  | ![設定為由下至上的雙窗格檢視](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>MinWideModeWidth 和 MinTallModeHeight 的特殊值

您可以使用 `MinWideModeWidth` 屬性來防止雙窗格檢視進入寬模式 - 只要將 `MinWideModeWidth` 設定為 [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) 即可。

如果您將 `MinTallModeHeight` 設定為 [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0)，雙窗格檢視即無法進入高模式。

如果您將 `MinTallModeHeight` 設定為 0，雙窗格檢視即無法進入 `SinglePane` 模式。

#### <a name="responding-to-mode-changes"></a>回應模式變更

您可以使用唯讀 [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) 屬性來取得目前的顯示模式。 每當雙窗格檢視變更其顯示的窗格時，就會發生 [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged)事件，然後才轉譯更新的內容。 您可以處理事件，以回應顯示模式中的變更。

> [!TIP]
> 第一次載入頁面時並不會發生 `ModeChanged` 事件，因此您的預設 XAML 應該會將 UI 呈現為其初次載入時的外觀。

您可以使用此事件的其中一種方式，就是更新應用程式的 UI，讓使用者可以在 `SinglePane` 模式中檢視所有內容。 例如，範例應用程式具有主要窗格 (影像) 和資訊窗格。

![範例應用程式跨高模式時的小影像](images/two-pane-view/tpv-top-bottom.png)

> _高模式_

當空間只能夠顯示一個窗格時，您可以將 `Pane2` 的內容移至 `Pane1`，讓使用者可藉由捲動查看所有內容。 它的外觀如下。

![此影像顯示一個螢幕上的範例應用程式，其中包含在單一窗格中捲動的所有內容](images/two-pane-view/tpv-single-pane.png)

> _SinglePane 模式_

請記住，`MinWideModeWidth` 和 `MinTallModeHeight` 屬性會決定顯示模式變更的時間，因此您可以藉由調整這些屬性的值，來變更何時要在窗格之間移動內容。

以下是在 `Pane1` 和 `Pane2` 之間移動內容的 `ModeChanged` 事件處理常式程式碼。 這也會設定 [VisualState](/uwp/api/windows.ui.xaml.visualstate) 以限制寬模式中的影像寬度。

```csharp
private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
{
    // Remove details content from it's parent panel.
    ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
    // Set Normal visual state.
    Windows.UI.Xaml.VisualStateManager.GoToState(this, "Normal", true);

    // Single pane
    if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
    {
        // Add the details content to Pane1.
        Pane1StackPanel.Children.Add(DetailsContent);
    }
    // Dual pane.
    else
    {
        // Put details content in Pane2.
        Pane2Root.Children.Add(DetailsContent);

        // If also in Wide mode, set Wide visual state
        // to constrain the width of the image to 2*.
        if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
        {
            Windows.UI.Xaml.VisualStateManager.GoToState(this, "Wide", true);
        }
    }
}
```

## <a name="dos-and-donts"></a>可行與禁止事項

- 請盡可能使用雙窗格檢視，讓應用程式可以利用雙螢幕和大型螢幕的優勢。
- 不要將兩個窗格檢視放在另一個兩個窗格檢視中。

## <a name="related-articles"></a>相關文章

- [版面配置概觀](../layout/index.md)
- [雙螢幕開發](/dual-screen)
- [雙螢幕裝置簡介](/dual-screen/introduction)
