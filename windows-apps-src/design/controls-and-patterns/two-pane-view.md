---
Description: TwoPaneView 是一種版面配置控制項，可協助您管理具有 2 個不同內容區域的應用程式顯示。
title: 兩個窗格檢視
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b3d12f2aad1d5dffbbad0790e5940699536daf0b
ms.sourcegitcommit: e6a435716799c7bb192b3d5c4d3b8295ec3911d4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2020
ms.locfileid: "76549703"
---
# <a name="two-pane-view"></a>兩個窗格檢視

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) 是一種版面配置控制項，可協助您管理具有 2 個不同內容區域的應用程式顯示，例如主要/詳細資料檢視。

> [!IMPORTANT]
> 本文說明處於公開預覽狀態的功能和指導方針，在正式推出之前可能會大幅修改。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

雖然它適用於所有 Windows 裝置，但是 TwoPaneView 控制項的設計目的是協助您自動充分利用雙螢幕裝置，而不需要特殊的編碼。 在雙螢幕裝置上，兩個窗格檢視可確保使用者介面 (UI) 在跨越螢幕之間的間隙時清楚地分割，如此一來，您的內容就會顯示在間距的任一側。

> **注意：** _雙螢幕裝置_是一種特殊裝置，具有獨特的功能。 這不等同於具有多個監視器的桌面裝置。 如需雙螢幕裝置的詳細資訊，請參閱[雙螢幕裝置簡介](/dual-screen/introduction)。
>
>在本文中，我們會使用_單螢幕裝置_或_單螢幕顯示_字詞來表示不是雙螢幕裝置的任何裝置，不論是單一監視器或多監視器設定的一部分。 TwoPaneView 控制項會以與其他 XAML 控制項相同的方式，在單一螢幕上運作。 如需您可以針對多個監視器最佳化應用程式的方式詳細資訊，請參閱[顯示多重檢視](/windows/uwp/design/layout/show-multiple-views)。

| **取得 Windows UI 程式庫** |
| - |
| 此控制項包含在 Windows UI 程式庫中；此程式庫是包含適用於 UWP 應用程式的新控制項和 UI 功能的 NuGet 封裝。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫概觀](/uwp/toolkits/winui/)。 |

| **平台 API** | **Windows UI 程式庫 API** |
| - | - |
| [TwoPaneView 類別](/uwp/api/windows.ui.xaml.controls.twopaneview) | [TwoPaneView 類別](/uwp/api/microsoft.ui.xaml.controls.twopaneview) |

在這整份文件中，我們將使用 XAML 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已將此新增至我們的[網頁](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page)元素：

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

在後方的程式碼中，我們也將使用 C# 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已在檔案頂端新增了此 **using** 陳述式：

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

當您有 2 個不同的內容區域時，使用兩個窗格檢視：

- 內容應該會自動重新排列並調整大小，以最適合視窗。
- 內容的次要區域應該根據可用空間顯示/隱藏。
- 內容應該在雙螢幕裝置的 2 個畫面之間清楚地分割。

## <a name="examples"></a>範例

這些影像會顯示在單一螢幕裝置和雙螢幕裝置上執行的應用程式。 兩個窗格檢視會將應用程式 UI 調適為每個裝置上的各種窗格設定。

![tpv-single.png](images/two-pane-view/tpv-single.png)

_單一螢幕裝置上的應用程式。_

![tpv-dual-wide.png](images/two-pane-view/tpv-dual-wide.png)

_跨雙螢幕裝置的應用程式 (寬模式)。_

![tpv-dual-tall.png](images/two-pane-view/tpv-dual-tall.png)

_跨雙螢幕裝置的應用程式 (高模式)。_

## <a name="how-it-works"></a>運作方式

兩個窗格檢視有兩個窗格，您可以在其中放置內容。 會根據視窗的可用空間來調整窗格的大小和排列。 可能的窗格版面配置是由 [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode) 列舉所定義：

- **SinglePane** - 只會顯示一個窗格，如 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 屬性所指定。
- **寬** - 並排顯示窗格，或顯示單一窗格，如 [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) 屬性所指定。
- **高** - 上下顯示窗格，或顯示單一窗格，如 [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) 屬性所指定。

您可以藉由設定 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 以指定當只有一個窗格的空間時，要顯示哪一個窗格，以設定兩個窗格檢視。 然後，您可以指定 Pane1 顯示在高視窗的上方或下方，或是在寬視窗的左邊或右邊。

兩個窗格檢視會處理窗格的大小和排列，但是您仍然需要因應大小和方向的變更來調適窗格內的內容。 如需有關建立調適型 UI 的詳細資訊，請參閱[搭配 XAML 的回應式版面配置](/windows/uwp/design/layout/layouts-with-xaml)和[版面配置面板](/windows/uwp/design/layout/layout-panels)。

兩個窗格檢視會根據應用程式執行所在的裝置類型來管理窗格的顯示：

- 在雙螢幕裝置上

    兩個窗格檢視的設計可讓您輕鬆地最佳化雙螢幕裝置的 UI。 視窗會自行調整大小，以使用螢幕上的所有可用空間。 當您的應用程式只在裝置的其中一個螢幕上時，會顯示一個窗格，如 [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) 屬性所指定。

    當您的應用程式跨越雙螢幕裝置的兩個螢幕時，每個螢幕會顯示其中一個窗格的內容，並且讓內容適當地跨越間距。 當您使用兩個窗格檢視時，會內建跨越感知功能。 您只需要設定高/寬設定，以指定要在哪一個螢幕上顯示哪一個窗格。 兩個窗格檢視會負責處理其餘部分。


- 在單一螢幕裝置上

    在單一螢幕裝置 (例如膝上型電腦或桌上型電腦) 上執行時，兩個窗格檢視會提供您預期的任何 XAML 控制項行為。 當視窗重新調整大小時，兩個窗格檢視會根據視窗大小調整其窗格的大小和位置。 它具有額外屬性，您可加以設定，以定義在可重新調整大小的視窗中於單一螢幕上顯示時，控制項的行為。 在下一節中，我們會更詳細地說明這些屬性。

## <a name="how-to-use-the-two-pane-view-control"></a>如何使用兩個窗格檢視控制項

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) 不一定要是您頁面版面配置的根元素。 事實上，您通常會在提供應用程式整體導覽的 [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) 控制項內使用它。 TwoPaneView 會適當地調整，不論它在 XAML 樹狀結構中的哪個位置；不過，我們建議您不要在另一個 TwoPaneView 內讓 TwoPaneView 成為巢狀。

### <a name="add-content-to-the-panes"></a>將內容新增至窗格

兩個窗格檢視的每個窗格都可以保存單一 XAML UIElement。 若要新增內容，您通常會在每個窗格中放置 XAML 版面配置面板，然後將其他控制項和內容新增至面板。 窗格可以變更大小，並且在寬和高模式之間切換，因此您必須確定每個窗格中的內容都可以適應這些變更。 如需有關建立調適型 UI 的詳細資訊，請參閱[搭配 XAML 的回應式版面配置](/windows/uwp/design/layout/layouts-with-xaml)和[版面配置面板](/windows/uwp/design/layout/layout-panels)。

這個範例會建立簡單的圖片/資訊應用程式 UI，如下所示。 當有兩個窗格的空間時，圖片和資訊會顯示在不同的窗格中。 (當只有一個窗格的空間時，您會將 Pane2 的內容移至 Pane1，並且讓使用者捲動以查看任何隱藏的內容。 您稍後會在_回應模式變更_一節中看到這個部分的程式碼。)

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

```xaml
<muxc:TwoPaneView
    Pane1Length="*"
    ModeChanged="TwoPaneView_ModeChanged">

    <muxc:TwoPaneView.Pane1>
        <Grid x:Name="Pane1Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <CommandBar x:Name="MyCommandBar" DefaultLabelPosition="Right">
                <AppBarButton x:Name="Share" Icon="Share" Label="Share"/>
                <AppBarButton x:Name="Print" Icon="Print" Label="Print"/>
            </CommandBar>

            <ScrollViewer Grid.Row="1">
                <StackPanel x:Name="Pane1StackPanel">
                    <Image x:Name="TheImage" Source="Assets\LandscapeImage8.jpg"
                           VerticalAlignment="Top" HorizontalAlignment="Center" 
                           Margin="16,0"/>
                </StackPanel>
            </ScrollViewer>

        </Grid>
    </muxc:TwoPaneView.Pane1>

    <muxc:TwoPaneView.Pane2>
        <Grid x:Name="Pane2Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="DetailsContent" Grid.Row="1"
                Orientation="Vertical" Padding="16">

                <TextBlock Text="Mountain.jpg" Margin="0,0,0,12"
                   FontWeight="SemiBold" FontSize="18"/>

                <TextBlock Text="Date Taken:" FontWeight="SemiBold"/>
                <TextBlock Text="8/29/2019 9:55am" Margin="0,0,0,12"/>

                <TextBlock Text="Dimensions:" FontWeight="SemiBold"/>
                <TextBlock Text="1000x750" Margin="0,0,0,12"/>

                <TextBlock Text="Resolution:" FontWeight="SemiBold"/>
                <TextBlock Text="96 dpi" Margin="0,0,0,12"/>

                <TextBlock Text="Description:" FontWeight="SemiBold"/>
                <TextBlock TextWrapping="Wrap" 
                           Text="Lorem ipsum dolor sit amet."/>
            </StackPanel>
        </Grid>
    </muxc:TwoPaneView.Pane2>
</muxc:TwoPaneView>
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

單一螢幕裝置上的窗格大小取決於 [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) 和 [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length) 屬性。 這些屬性會使用 [GridLength](/uwp/api/windows.ui.xaml.gridlength) 值，支援_自動_和_星號_(\*)調整大小。 如需自動和以星號調整大小的說明，請參閱[搭配 XAML 的回應式版面配置](/windows/uwp/design/layout/layouts-with-xaml#layout-properties)的_版面配置屬性_一節。

根據預設，Pane1Length 會設定為**自動**，而且會自行調整大小以符合其內容。 Pane2Length 設定為 *，它會使用所有剩餘的空間。

![tpv-size-default.png](images/two-pane-view/tpv-size-default.png)

_具有預設大小的窗格_

預設值適用於一般主要/詳細資料版面配置，其中您在 Pane1 中有項目清單，在 Pane2 中有許多詳細資料。 不過，視您的內容而定，您可能會想要以不同的方式分割空間。 在這裡，Pane1Length 會設定為 2*，因此它會取得 Pane2 的兩倍空間。

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![tpv-size-2.png](images/two-pane-view/tpv-size-2.png)

_窗格大小 2* 和 *_

> **注意：** 如先前所述，在雙螢幕裝置上會忽略這些屬性，而且窗格會根據裝置_狀態_自動調整大小和排列。

如果您將窗格設定為使用自動調整大小，您可以藉由設定保留窗格內容的面板高度和寬度，來控制大小。 在此情況下，您可能需要處理 ModeChanged 事件，並將內容的高度和寬度條件約束設定為適用於目前的模式。

### <a name="display-in-wide-or-tall-mode"></a>以寬或高模式顯示

在桌面顯示器上，兩個窗格檢視的顯示[模式](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode)取決於 [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) 和 [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight) 屬性。 這兩個屬性的預設值都是641px，與 [NavigationView. CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) 相同。

如果視窗是：

- 寬於 MinWideModeWidth，則會使用 **Wide** 模式。
- 窄於 MinWideModeWidth，但是高於 MinTallModeHeight，則會使用 **Tall** 模式。
- 窄於 MinWideModeWidth，並且短於 MinTallModeHeight，則會使用 **SinglePane** 模式。

> **注意：** 如先前所述，在雙螢幕裝置上會忽略這些屬性，而且窗格會根據裝置_狀態_自動調整大小和排列。

#### <a name="wide-configuration-options"></a>寬設定選項

當單一顯示器寬於 MinWideModeWidth 屬性時，兩個窗格檢視會進入寬模式。 MinWideModeWidth 控制兩個窗格檢視進入寬模式的時機。 預設值為 641px，但是您可以將它變更為您想要的任何值。 一般來說，您應該將此屬性設定為您希望窗格成為最小寬度的任何值。

當兩個窗格檢視處於寬模式時，WideModeConfiguration 屬性會決定要顯示的內容：

- **SinglePane** - 單一窗格 (由 PanePriority 決定)。 此窗格會佔用 TwoPaneView 的完整大小 (亦即，兩個方向都是以星號調整大小)。
- **LeftRight** - Pane1 在左側/Pane2 在右側。 兩個窗格都是以垂直方向以星號調整大小，Pane1 的寬度是自動調整，而 Pane2 的寬度則是以星號調整大小。
- **RightLeft** - Pane1 在右側/Pane2 在左側。 兩個窗格都是以垂直方向以星號調整大小，Pane2 的寬度是自動調整，而 Pane1 的寬度則是以星號調整大小。

預設設定是 **LeftRight**。

| LeftRight | RightLeft |
| - | - |
| ![tpv-left-right.png](images/two-pane-view/tpv-left-right.png)  | ![tpv-right-left.png](images/two-pane-view/tpv-right-left.png)  |

> **提示：** 當裝置使用由右至左 (RTL) 語言時，兩個窗格檢視會自動交換順序：RightLeft 會轉譯為 LeftRight，而 LeftRight 會轉譯為 RightLeft。

#### <a name="tall-configuration-options"></a>高設定選項

當單一顯示器窄於 MinWideModeWidth，且高於 MinTallModeHeight 時，兩個窗格檢視會進入高模式。 預設值為 641px，但是您可以將它變更為您想要的任何值。 一般來說，您應該將此屬性設定為您希望窗格成為最小高度的任何值。

當兩個窗格檢視處於寬模式時，TallLayout 屬性會決定要顯示的內容：

- **SinglePane** - 單一窗格 (由 PanePriority 決定)。 此窗格會佔用 TwoPaneView 的完整大小 (亦即，兩個方向都是以星號調整大小)。
- **TopBottom** - Pane1 在上方/Pane2 在右側。 兩個窗格都是以水平方向以星號調整大小，Pane1 的高度是自動調整，而 Pane2 的高度則是以星號調整大小。
- **BottomTop** - Pane1 在右側/Pane2 在左側。 兩個窗格都是以水平方向以星號調整大小，Pane2 的高度是自動調整，而 Pane1 的高度則是以星號調整大小。

預設值是 **TopBottom**。

| TopBottom | BottomTop |
| - | - |
| ![tpv-top-bottom.png](images/two-pane-view/tpv-top-bottom.png)  | ![tpv-bottom-top.png](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>MinWideModeWidth 和 MinTallModeHeight 的特殊值

您可以使用 MinWideModeWidth 屬性來防止兩個窗格檢視進入寬模式 - 只要將 MinWideModeWidth 設定為 [PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0)。

如果您將 MinTallModeHeight 設定為 [PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0)，會防止兩個窗格檢視進入高模式。

如果您將 MinTallModeHeight 設定為 0，會防止兩個窗格檢視進入 SinglePane 模式。

#### <a name="responding-to-mode-changes"></a>回應模式變更

您可以使用唯讀 [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) 屬性來取得目前的顯示模式。 每當兩個窗格檢視變更其顯示的窗格時，就會發生 [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged)事件，然後才轉譯更新的內容。 您可以處理事件，以回應顯示模式中的變更。

您可以使用此事件的其中一種方式，就是更新應用程式的 UI，讓使用者可以在 SinglePane 模式中檢視所有內容。 例如，範例應用程式具有主要窗格 (影像) 和資訊窗格。

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

_寬模式_

當空間只能夠顯示一個窗格時，您可以將 Pane2 的內容移至 Pane1，讓使用者可以捲動來查看所有內容。 它的外觀如下。

![tpv-mode-change.png](images/two-pane-view/tpv-mode-change.png)

_SinglePane 模式_

```csharp
 private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
 {
     ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
     ((Panel)MyCommandBar.Parent).Children.Remove(MyCommandBar);

     // Single pane
     if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
     {
         // Add the command bar and details content to Pane1.
         Pane1StackPanel.Children.Add(DetailsContent);
         Pane1Root.Children.Add(MyCommandBar);
     }
     // Dual pane.
     else
     {
         // Wide mode.
         if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
         {
             // Put the command bar in Pane2.
             Pane2Root.Children.Add(MyCommandBar);
         }
         // Tall mode.
         else if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Tall)
         {
             // Put the command bar in Pane1
             Pane1Root.Children.Add(MyCommandBar);
         }

         // Put details content in Pane2.
         Pane2Root.Children.Add(DetailsContent);
     }
 }
```

## <a name="dos-and-donts"></a>可行與禁止事項

- 請在可行時使用兩個窗格檢視，讓應用程式可以利用雙顯示器和大型螢幕的優勢。
- 不要將兩個窗格檢視放在另一個兩個窗格檢視中。

## <a name="related-articles"></a>相關文章

- [版面配置概觀](../layout/index.md)
- [雙螢幕開發](/dual-screen)
- [雙螢幕裝置簡介](/dual-screen/introduction)
