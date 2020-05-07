---
Description: 對話方塊和飛出視窗會在使用者要求暫時性 UI 元素，或發生需要通知或核准的情況時顯示暫時性 UI 元素。
title: 飛出視窗控制項
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 320586fb8fe7f71eaea2d4b12c0dd731a1f721db
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "80080973"
---
# <a name="flyouts"></a>飛出視窗

飛出視窗是一個會消失關閉的容器，可顯示任意 UI 作為其內容。 飛出視窗可包含其他飛出視窗或操作功能表，以建立巢狀體驗。

![巢狀內嵌於飛出視窗內的操作功能表](../images/flyout-nested.png)

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](../images/winui-logo-64x64.png) | Windows UI 程式庫 2.2 或更新版本中有這個控制項使用圓角的新範本。 如需詳細資訊，請參閱[圓角半徑](/windows/uwp/design/style/rounded-corner)。 WinUI 是 NuGet 套件，包含適用於 UWP 應用程式的新控制項與 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **平台 API：** [Flyout 類別](/uwp/api/Windows.UI.Xaml.Controls.Flyout)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

* 不要使用飛出視窗來取代[工具提示](../tooltips.md)或[操作功能表](../menus.md)。 使用工具提示來顯示會在特定時間之後隱藏的簡短說明。 使用操作功能表來執行與 UI 元素相關聯的內容相關動作，例如複製和貼上。

如需使用飛出視窗和使用對話方塊 (類似的控制項) 之時機的建議，請參閱[對話方塊和飛出視窗](index.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡開啟應用程式並查看 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 或 <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> 運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

##  <a name="how-to-create-a-flyout"></a>如何建立飛出視窗


飛出視窗會附加至特定的控制項。 您可以使用 [Placement](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) 屬性來指定飛出視窗的顯示位置︰Top、Left、Bottom、Right 或 Full。 如果您選取[「Full」位置模式](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode)，App 會延展飛出視窗，並將它置於 App 視窗的中央。 有些控制項 (例如，[Button](/uwp/api/Windows.UI.Xaml.Controls.Button)) 會提供可用來與飛出視窗或[操作功能表](../menus.md)產生關聯的 [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) 屬性。

這個範例會建立簡單的飛出視窗，並在按下按鈕時顯示一些文字。
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

如果控制項沒有 flyout 屬性，您可以改為使用 [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.AttachedFlyoutProperty) 附加屬性。 當您這樣做時，也需要呼叫 [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase#Windows_UI_Xaml_Controls_Primitives_FlyoutBase_ShowAttachedFlyout_Windows_UI_Xaml_FrameworkElement_) 方法以顯示飛出視窗。

這個範例會將簡單的飛出視窗新增至影像。 當使用者點選影像時，App 會顯示飛出視窗。

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50"
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock Text="This is some text in a flyout."  />
    </Flyout>
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

先前的範例會以內嵌方式定義飛出視窗。 您也可以將飛出視窗定義為靜態資源，然後和多個元素搭配使用。 這個範例會建立更複雜的飛出視窗，並會在點選影像縮圖時顯示該影像的大尺寸版本。

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}"
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

## <a name="style-a-flyout"></a>設定飛出視窗樣式
若要設定飛出視窗的樣式，請修改其 [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle)。 這個範例會顯示一段換行的文字，並使文字區塊可供螢幕助讀程式存取。

![包含換行文字的協助工具飛出視窗](../images/flyout-wrapping-text.png)

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode"
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

## <a name="styling-flyouts-for-10-foot-experiences"></a>設定 10 英呎體驗的飛出視窗樣式

像飛出視窗這樣的消失關閉控制項會將鍵盤和遊戲台焦點圈限在暫時性 UI 內，直到控制項關閉為止。 若要提供此行為的視覺提示，Xbox 上的消失關閉控制項將會繪製重疊，以使超出範圍 UI 的對比度和可見度變暗。 此行為可以使用 [`LightDismissOverlayMode`](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode) 屬性進行修改。 根據預設，飛出視窗將會在 Xbox 上繪製消失關閉重疊，但不會在其他裝置系列上繪製，不過應用程式可以選擇將重疊強制為一律為 **On** 或一律為 **Off**。

![包含變暗重疊的飛出視窗](../images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

## <a name="light-dismiss-behavior"></a>消失關閉行為
飛出視窗可以使用快速消失關閉動作來關閉，動作包括
-    點選飛出視窗外部
-    按下 Esc 鍵盤按鍵
-    按下硬體或軟體系統 [上一頁] 按鈕
-    按下遊戲台 B 按鈕

以點選來關閉時，這個手勢通常會沒入而無法傳遞至下面的 UI。 舉例說，如果已開啟的飛出視窗後面有可見的按鈕，使用者的第一個點選動作會關閉飛出視窗，但不會啟動此按鈕。 按下按鈕需要第二次點選。

您可以指定按鈕作為飛出視窗的輸入傳遞元素，來變更此行為。 飛出視窗將會因為上述消失關閉動作而關閉，還會將點選事件傳遞至其所指定的 `OverlayInputPassThroughElement`。 請考慮採用此行為，在功能類似的項目上加速使用者互動。 如果您的應用程式有我的最愛集合，而且集合中的每個項目都包含附加飛出視窗時，您可以合理預料使用者可能會希望與接連多個飛出視窗進行互動。

[!NOTE] 請小心，不要指定會產生破壞性動作的重疊輸入傳遞元素。 使用者已經習慣於不會啟用主要 UI 的慎重消失關閉動作。 不應讓 [關閉]、[刪除] 或類似具破壞性的按鈕在消失關閉時啟動，以免發生非預期和破壞性的行為。

在下列範例中，FavoritesBar 中所有三個按鈕都會在第一次點選時啟動。

````xaml
<Page>
    <Page.Resources>
        <Flyout x:Name="TravelFlyout" x:Key="TravelFlyout"
                OverlayInputPassThroughElement="{x:Bind FavoritesBar}">
            <StackPanel>
                <HyperlinkButton Content="Washington Trails Association"/>
                <HyperlinkButton Content="Washington Cascades - Go Northwest! A Travel Guide"/>
            </StackPanel>
        </Flyout>
    </Page.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="FavoritesBar" Orientation="Horizontal">
            <HyperlinkButton x:Name="PageLinkBtn">Bing</HyperlinkButton>
            <Button x:Name="Folder1" Content="Travel" Flyout="{StaticResource TravelFlyout}"/>
            <Button x:Name="Folder2" Content="Entertainment" Click="Folder2_Click"/>
        </StackPanel>
        <ScrollViewer Grid.Row="1">
            <WebView x:Name="WebContent"/>
        </ScrollViewer>
    </Grid>
</Page>
````
````csharp
private void Folder2_Click(object sender, RoutedEventArgs e)
{
     Flyout flyout = new Flyout();
     flyout.OverlayInputPassThroughElement = FavoritesBar;
     ...
     flyout.ShowAt(sender as FrameworkElement);
{
````

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章
- [工具提示](../tooltips.md)
- [功能表和操作功能表](../menus.md)
- [Flyout 類別](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog 類別](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)