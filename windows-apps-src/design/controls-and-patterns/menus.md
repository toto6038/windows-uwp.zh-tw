---
author: mijacobs
Description: A flyout is a lightweight popup that is used to temporarily show UI that is related to what the user is currently doing.
title: 功能表和操作功能表
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e38e9d61e8546d412cc30bad26680243f3a188e4
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2792341"
---
# <a name="menus-and-context-menus"></a>功能表和操作功能表



功能表和操作功能表會在使用者要求命令或選項時，顯示它們的清單。

> **重要 API**：[MenuFlyout 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout)、[ContextFlyout 屬性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx)、[FlyoutBase.AttachedFlyout 屬性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx)

![一般操作功能表的範例](images/contextmenu_rs2_icons.png)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？
功能表和操作功能表可組織命令並在使用者不需要它們時加以隱藏，藉以節省空間。 如果經常會用到某個特定命令，而且您有可用的空間，請考慮直接將它放置於它自己的元素中，而不是放在功能表中，讓使用者不需瀏覽功能表，即可取得該命令。

功能表和操作功能表適用於組織命令。若要顯示任意內容 (例如通知或要求確認)，請使用[對話方塊或飛出視窗](dialogs.md)。  

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/MenuFlyout">開啟應用程式並查看 MenuFlyout 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>功能表與操作功能表

功能表和操作功能表在其外觀和可包含的內容部分是完全相同的。 事實上，您會使用同一個控制項 [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) 來建立它們。 唯一的差別在於使用者存取它的方式。

您何時應該使用功能表或操作功能表？
* 如果裝載元素為按鈕或一些其他命令元素，且其主要作用是呈現其他命令，請使用功能表。
* 如果裝載元素是一些具有不同主要用途 (例如呈現文字或影像) 的其他類型元素，請使用操作功能表。

例如，在瀏覽窗格的按鈕上使用功能表，來提供額外的瀏覽選項。 在這個案例中，按鈕控制項的主要用途是提供功能表的存取。

![郵件中功能表的範例](images/Mail_Menu.png)

如果您想要在文字元素中新增命令 (例如剪下、複製及貼上)，請使用操作功能表，而不是功能表。 在這個案例中，文字元素的主要作用是呈現和編輯文字；其他命令 (例如剪下、複製及貼上) 均為次要的且隸屬於操作功能表。

![影像中心的操作功能表範例](images/ContextMenu_example.png) 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>功能表</b></p>
<ul>
<li>具有一律顯示的單一進入點 (例如，位於畫面頂端的 [檔案] 功能表)。</li>
<li>通常會附加到按鈕或父功能表項目。</li>
<li>是透過按一下滑鼠左鍵 (或對等的動作，例如使用手指點選) 來叫用。</li><li>透過 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx">Flyout</a> 或 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx">FlyoutBase.AttachedFlyout</a> 屬性來與元素產生關聯。</li>
</ul>
</div>
  <div class="side-by-side-content-right">
   <p><b>操作功能表</b></p>

<ul>
<li>已連結至單一元素，並會顯示次要命令。</li>
<li>是透過按一下滑鼠右鍵 (或對等的動作，例如使用您的手指長按) 來叫用。</li><li>透過其 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx">ContextFlyout</a> 屬性來與元素產生關聯。</li>
</ul>
  </div>
</div>
</div>

## <a name="icons"></a>圖示

請考慮將功能表項目圖示提供給：

<ul>
<li> 最常使用的項目 </li>
<li> 圖示為標準或眾所周知的功能表項目 </li>
<li> 圖示清楚表明命令所做動作的功能表項目 </li>
</ul>

不一定要為沒有標準視覺效果的命令提供圖示。 含義模糊的圖示沒有幫助，會造成顯示畫面凌亂，而且讓使用者無法專注在重要的功能表項目。

![使用圖示的範例操作功能表](images/contextmenu_rs2_icons.png)

````xaml
<MenuFlyout>
  <MenuFlyoutItem Text="Share" >
    <MenuFlyoutItem.Icon>
      <FontIcon Glyph="&#xE72D;" />
    </MenuFlyoutItem.Icon>
  </MenuFlyoutItem>
  <MenuFlyoutItem Text="Copy" Icon="Copy" />
  <MenuFlyoutItem Text="Delete" Icon="Delete" />
  <MenuFlyoutSeparator />
  <MenuFlyoutItem Text="Rename" />
  <MenuFlyoutItem Text="Select" />
</MenuFlyout>
````
> MenuFlyoutItems 中的圖示大小為 16x16px。 如果您使用 SymbolIcon、FontIcon 或 PathIcon，圖示會自動縮放成正確大小，但逼真度沒有降低。 如果使用 BitmapIcon，請確定您的資產為 16x16px。  

## <a name="create-a-menu-or-a-context-menu"></a>建立功能表或操作功能表

若要建立功能表或操作功能表，您可以使用 [MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)。 您可以藉由將 [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx)、[ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 及 [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) 物件新增到 MenuFlyout，來定義功能表的內容。 這些物件的用途如下：
* [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx) - 執行立即的動作。
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) - 開啟或關閉選項。
* [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) - 在視覺上分隔功能表項目。


這個範例會建立 [MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)，並使用 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 屬性 (適用於大多數控制項的屬性)，以將 [MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)顯示為操作功能表。

````xaml
<Rectangle
  Height="100" Width="100">
  <Rectangle.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </Rectangle.ContextFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

下一個範例幾乎完全相同，但不會使用 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 屬性來將 [MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)顯示為操作功能表，這個範例改用 [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 屬性來將它顯示為功能表。

````xaml
<Rectangle
  Height="100" Width="100"
  Tapped="Rectangle_Tapped">
  <FlyoutBase.AttachedFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </FlyoutBase.AttachedFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void Rectangle_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}

private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````


> 消失關閉控制項 (例如，功能表、操作功能表及其他飛出視窗) 會將鍵盤和遊戲台焦點困在暫時性 UI 內，直到關閉為止。 若要提供此行為的視覺提示，Xbox 上的消失關閉控制項將會繪製重疊，以使超出範圍 UI 的可見度變暗。 您可以使用新的 [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx) 屬性來修改此行為。 根據預設，暫時性 UI 將在 Xbox (**\[自動\]**) 上繪製消失關閉重疊，但不會在其他裝置系列上繪製，不過應用程式可以選擇強制重疊一律為 **\[開啟\]** 或一律為 **\[關閉\]**。

> ```xaml
> <MenuFlyout LightDismissOverlayMode="Off" />
> ```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)：以互動式格式查看所有 XAML 控制項。
- [XAML 操作功能表範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>相關文章

- [MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)
