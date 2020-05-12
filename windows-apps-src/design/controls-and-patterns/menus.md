---
Description: 功能表和操作功能表會在使用者要求命令或選項時，顯示它們的清單。
title: 功能表和操作功能表
label: Menus and context menus
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: RS5, 19H1
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 884bd2e1eded5e3c0dfa92f1488fe0b8661af18d
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970353"
---
# <a name="menus-and-context-menus"></a>功能表和操作功能表

功能表和操作功能表會在使用者要求命令或選項時，顯示它們的清單。 使用功能表飛出視窗來顯示單一的內嵌功能表。 使用功能表列來以水平列的形式顯示一組功能表，通常會顯示在應用程式視窗的頂端。 每個功能表都可以有功能表項目和子功能表。

![一般操作功能表的範例](images/contextmenu_rs2_icons.png)

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | 此 **MenuBar** 控制項包含在 Windows UI 程式庫中；該程式庫是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫概觀](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **Windows UI 程式庫 API：** [MenuBar 類別](/uwp/api/microsoft.ui.xaml.controls.menubar) \(英文\)
>
> **平台 API：** [MenuFlyout 類別](/uwp/api/windows.ui.xaml.controls.menuflyout)、[MenuBar 類別](/uwp/api/windows.ui.xaml.controls.menubar)、[ContextFlyout 屬性](/uwp/api/windows.ui.xaml.uielement.contextflyout)、[FlyoutBase.AttachedFlyout 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties) \(英文\)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

功能表和操作功能表可組織命令並在使用者不需要它們時加以隱藏，藉以節省空間。 如果經常會用到某個特定命令，而且您有可用的空間，請考慮直接將它放置於它自己的元素中，而不是放在功能表中，讓使用者不需瀏覽功能表，即可取得該命令。

功能表和操作功能表適用於組織命令；若要顯示任意內容 (例如通知或確認要求)，請使用[對話方塊或飛出視窗](dialogs.md)。

### <a name="menubar-vs-menuflyout"></a>MenuBar 與MenuFlyout

若要在飛出視窗中顯示附加至畫布上 UI 元素的功能表，請使用 MenuFlyout 控制項來裝載功能表項目。 您可以將功能表飛出視窗叫用為一般功能表或操作功能表。 功能表飛出視窗會裝載單一最上層功能表 (以及選擇性的子功能表)。

使用 MenuBar 來以水平列的形式顯示多個最上層功能表的集合。 您通常會將功能表列置於應用程式視窗的頂端。

### <a name="menubar-vs-commandbar"></a>MenuBar 與CommandBar

MenuBar 和 CommandBar 皆代表您可以用來向使用者公開命令的表面。 MenuBar 能提供快速且簡單的方法，以針對可能需要進行比 CommandBar 所允許還要多的組織和分組的應用程式公開命令集合。

您也可以使用搭配 CommandBar 使用 MenuBar。 使用 MenuBar 來提供大部分的命令，並使用 CommandBar 來醒目提示最常使用的命令。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡以<a href="xamlcontrolsgallery:/item/MenuFlyout">開啟應用程式並查看 MenuFlyout 的運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>功能表與操作功能表

功能表和操作功能表的外觀和可包含的內容皆非常相似。 事實上，您會使用同一個控制項 [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout) \(英文\) 來建立它們。 它們唯一的差別在於使用者存取它的方式。

您何時應該使用功能表或操作功能表？

- 如果裝載元素為按鈕或一些其他命令元素，且其主要作用是呈現其他命令，請使用功能表。
- 如果裝載元素是一些具有不同主要用途 (例如呈現文字或影像) 的其他類型元素，請使用操作功能表。

例如，請針對為清單提供篩選及排序選項的按鈕使用功能表。 在這個案例中，按鈕控制項的主要用途是提供功能表的存取。

![[郵件] 中功能表的範例](images/Mail_Menu.png)

如果您想要在文字元素中新增命令 (例如剪下、複製及貼上)，請使用操作功能表，而不是功能表。 在這個案例中，文字元素的主要作用是呈現和編輯文字；其他命令 (例如剪下、複製及貼上) 均為次要的且隸屬於操作功能表。

![相片圖庫中操作功能表的範例](images/ContextMenu_example.png)

### <a name="menus"></a>功能表

- 具有一律顯示的單一進入點 (例如，位於畫面頂端的 [檔案] 功能表)。
- 通常會附加到按鈕或父功能表項目。
- 是透過按一下滑鼠左鍵 (或對等的動作，例如使用手指點選) 來叫用。
- 透過其 [Flyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button.flyout) \(英文\) 或 [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties) \(英文\) 屬性來與元素產生關聯，或是分組在應用程式視窗頂端的功能表列中。

### <a name="context-menus"></a>操作功能表

- 連結至單一元素，並會顯示次要命令。
- 透過以滑鼠右鍵按一下 (或是對等動作，例如使用手指長按) 來叫用。
- 透過其 [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) 屬性來與元素產生關聯。

## <a name="icons"></a>圖示

考慮將功能表項目圖示提供給：

- 最常使用的項目。
- 其圖示為標準或眾所周知的功能表項目。
- 其圖示能清楚表明命令所做動作的功能表項目。

不要覺得一定要為沒有標準視覺效果的命令提供圖示。 含義模糊的圖示本身並沒有幫助、會使顯示畫面凌亂，也會讓使用者無法專注在重要的功能表項目上。

![具有圖示的範例操作功能表](images/contextmenu_rs2_icons.png)

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

> [!TIP]
> MenuFlyoutItem 中的圖示大小為 16x16px。 如果您使用 SymbolIcon、FontIcon 或 PathIcon，圖示會自動縮放成正確大小，但不會失去逼真度。 如果您使用 BitmapIcon，請確定您的資產為 16x16px。  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>建立功能表飛出視窗或操作功能表

若要建立功能表飛出視窗或操作功能表，請使用 [MenuFlyout 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) \(英文\)。 您可以藉由將 [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem) \(英文\)、[MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem) \(英文\)、[ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) \(英文\)、[RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) \(英文\)，以及 [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) \(英文\) 物件新增到 MenuFlyout，來定義功能表的內容。

這些物件的用途如下：

- [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem) - 執行立即的動作。
- [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem) \(英文\) - 包含功能表項目的階層式清單。
- [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) - 開啟或關閉選項。
- [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)(英文\) - 在互斥功能表項目之間切換。
- [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) - 在視覺上分隔功能表項目。

這個範例會建立 [MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) \(英文\)，並使用 [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) \(英文\) 屬性 (適用於大多數控制項) 來將 MenuFlyout 顯示為操作功能表。

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

下一個範例幾乎完全相同，但不會使用 [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) 屬性來將 [MenuFlyout 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)顯示為操作功能表，這個範例改用 [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 屬性來將它顯示為功能表。

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

### <a name="light-dismiss"></a>消失關閉

消失關閉控制項 (例如功能表、操作功能表及其他飛出視窗) 會將鍵盤和遊戲控制器的焦點困在暫時性 UI 內，直到關閉為止。 若要提供此行為的視覺提示，Xbox 上的消失關閉控制項將會繪製重疊，以使超出範圍 UI 的可見度變暗。 此行為可以透過 [LightDismissOverlayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode) \(英文\) 屬性來加以修改。 根據預設，暫時性 UI 會在 Xbox 上繪製消失關閉重疊 ([自動]  )，但不會繪製到其他裝置系列上。 您可以選擇強制使重疊一律 [開啟]  或 [關閉]  。

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>建立功能表列

> [!IMPORTANT]
> MenuBar 需要 Windows 10 1809 版 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或是 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/) \(英文\)。

您會使用和功能表飛出視窗相同的元素來在功能表列中建立功能表。 不過，與其將 MenuFlyoutItem 物件分組到 MenuFlyout 中，您會將它們分組到 MenuBarItem 元素中。 每個 MenuBarItem 都會被新增至 MenuBar 中作為最上層功能表。

![功能表列的範例](images/menu-bar-submenu.png)

> [!NOTE]
> 此範例僅示範如何建立 UI 結構，但不會示範任何命令的實作。

```xaml
<muxc:MenuBar>
    <muxc:MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save"/>
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="View">
        <MenuFlyoutItem Text="Output"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Landscape" GroupName="OrientationGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Portrait" GroupName="OrientationGroup" IsChecked="True"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Small icons" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Medium icons" IsChecked="True" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Large icons" GroupName="SizeGroup"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </muxc:MenuBarItem>
</muxc:MenuBar>
```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。
- [XAML 操作功能表範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu) \(英文\)

## <a name="related-articles"></a>相關文章

- [MenuFlyout 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) \(英文\)
- [MenuBar 類別](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar) \(英文\)
