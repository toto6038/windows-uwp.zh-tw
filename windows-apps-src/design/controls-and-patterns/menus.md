---
Description: 功能表和操作功能表會在使用者要求命令或選項時，顯示它們的清單。
title: 功能表和操作功能表
label: Menus and context menus
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: RS5, 19H1
keywords: Windows 10, UWP
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 10e91e8098f232d2875c802567674c9feacb2af9
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364623"
---
# <a name="menus-and-context-menus"></a>功能表和操作功能表

功能表和操作功能表會在使用者要求命令或選項時，顯示它們的清單。 用以顯示單一的內嵌功能表的功能表彈出式視窗。 若要顯示在水平列中，通常在應用程式視窗頂端的一組功能表使用功能表列。 每個功能表可以有功能表項目和子功能表。

![一般操作功能表的範例](images/contextmenu_rs2_icons.png)

| **取得 Windows 的 UI 程式庫** |
| - |
| 此控制項是包含 Windows UI 程式庫，包含新的控制項和 UWP 應用程式的 UI 功能的 NuGet 套件的過程。 如需詳細資訊，包括安裝指示，請參閱 < [Windows 的 UI 程式庫概觀](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

| **平台 Api** | **Windows UI 程式庫 Api** |
| - | - |
| [MenuFlyout 類別](/uwp/api/windows.ui.xaml.controls.menuflyout)， [MenuBar 類別](/uwp/api/windows.ui.xaml.controls.menubar)， [ContextFlyout 屬性](/uwp/api/windows.ui.xaml.uielement.contextflyout)， [FlyoutBase.AttachedFlyout 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout) | [功能表列類別](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

功能表和操作功能表可組織命令並在使用者不需要它們時加以隱藏，藉以節省空間。 如果經常會用到某個特定命令，而且您有可用的空間，請考慮直接將它放置於它自己的元素中，而不是放在功能表中，讓使用者不需瀏覽功能表，即可取得該命令。

功能表和內容功能表會用來組織命令的方式。若要顯示任意的內容，例如 「 通知 」 或 「 確認 」 要求中，使用[ 對話方塊或飛出視窗](dialogs.md)。

### <a name="menubar-vs-menuflyout"></a>功能表列 vs。MenuFlyout

若要顯示功能表飛出視窗附加至畫布上 UI 元素中，使用 MenuFlyout 控制項來裝載您的功能表項目。 作為一般的功能表或操作功能表，您可以叫用的功能表彈出式視窗。 功能表飛出視窗上裝載單一的最上層功能表 （和選擇性的子功能表）。

若要顯示水平資料列中的一組多個最上層的功能表，請使用功能表列。 您通常會放置在應用程式視窗頂端的功能表列。

### <a name="menubar-vs-commandbar"></a>功能表列 vs。CommandBar

功能表列和 CommandBar 都代表可供您的命令公開給您的使用者介面。 功能表列會提供快速且簡單的方式來公開一組命令可能需要更多的組織或群組比 CommandBar 所允許的應用程式。

您也可以使用功能表列搭配 CommandBar。 您可以使用功能表列，提供大量的命令和命令列來反白顯示最常使用的命令。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/MenuFlyout">開啟應用程式並查看 MenuFlyout 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>功能表與操作功能表

功能表和內容功能表會在它們的外觀，而且它們可以包含類似。 事實上，您可以使用相同的控制項[MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout)，來建立它們。 差別在於您要如何讓使用者存取它。

您何時應該使用功能表或操作功能表？

- 如果主機項目 按鈕或其主要角色是為了要呈現其他命令的一些其他命令元素，請使用功能表。
- 如果裝載元素是一些具有不同主要用途 (例如呈現文字或影像) 的其他類型元素，請使用操作功能表。

比方說，使用功能表上的按鈕，提供篩選和排序選項的清單。 在這個案例中，按鈕控制項的主要用途是提供功能表的存取。

![郵件中功能表的範例](images/Mail_Menu.png)

如果您想要在文字元素中新增命令 (例如剪下、複製及貼上)，請使用操作功能表，而不是功能表。 在這個案例中，文字元素的主要作用是呈現和編輯文字；其他命令 (例如剪下、複製及貼上) 均為次要的且隸屬於操作功能表。

![影像中心的操作功能表範例](images/ContextMenu_example.png)

### <a name="menus"></a>功能表

- 具有一律顯示的單一進入點 (例如，位於畫面頂端的 [檔案] 功能表)。
- 通常會附加到按鈕或父功能表項目。
- 是透過按一下滑鼠左鍵 (或對等的動作，例如使用手指點選) 來叫用。
- 相關聯的項目，透過其[飛出視窗](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button.flyout)或[FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout)屬性，或在功能表列頂端的 [應用程式] 視窗中分組。

### <a name="context-menus"></a>操作功能表

- 已連結至單一元素，並會顯示次要命令。
- 是透過按一下滑鼠右鍵 (或對等的動作，例如使用您的手指長按) 來叫用。
- 透過其 [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) 屬性來與元素產生關聯。

## <a name="icons"></a>圖示

請考慮將功能表項目圖示提供給：

- 最常使用的項目。
- 其圖示是標準或已知的功能表項目。
- 其圖示也會說明命令功能的功能表項目。

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

> [!TIP]
> MenuFlyoutItem 中圖示的大小是 16x16px。 如果您使用 SymbolIcon、 FontIcon 或 PathIcon，圖示將會自動調整至正確的大小，而不會遺失精確度。 如果使用 BitmapIcon，請確定您的資產為 16x16px。  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>建立功能表飛出視窗或操作功能表

若要建立的功能表彈出式視窗或操作功能表時，您使用[MenuFlyout 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)。 藉由加入定義功能表的內容[MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem)， [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem)， [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)， [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)並[MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) MenuFlyout 的物件。

這些物件的用途如下：

- [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem) - 執行立即的動作。
- [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem)— 包含階層式清單的功能表項目。
- [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) - 開啟或關閉選項。
- [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)— 互斥功能表項目之間切換。
- [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) - 在視覺上分隔功能表項目。

這個範例會建立[MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)並用[ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout)屬性，可用於大部分的控制項，以顯示操作功能表為 MenuFlyout 屬性。

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

### <a name="light-dismiss"></a>正常關閉

正常關閉功能表、 操作功能表和其他延伸顯示等控制項，設陷在暫時性 UI 之前關閉鍵盤及遊戲台焦點。 若要提供此行為的視覺提示，Xbox 上的消失關閉控制項將會繪製重疊，以使超出範圍 UI 的可見度變暗。 您可以使用新的 [LightDismissOverlayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode) 屬性來修改此行為。 根據預設，暫時性的 Ui 會在 Xbox 上繪製淺解除覆疊 (**自動**)，但不是其他裝置系列。 您可以強制覆疊可讓您隨時**上**或 alwayson**關閉**。

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>建立功能表列

> [!IMPORTANT]
> 功能表列需要 Windows 10 版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或有[Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

您可以使用相同的項目來建立在功能表列中，如所示的功能表彈出式視窗的功能表。 不過，而不是群組中 MenuFlyout MenuFlyoutItem 物件，您將它們分組在 MenuBarItem 項目。 每個 MenuBarItem 加入功能表列中，最上層功能表的方式。

![功能表列的範例](images/menu-bar-submenu.png)

> [!NOTE]
> 此範例示範如何只建立 UI 結構，但不會顯示的任何命令的實作。

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

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以互動式格式查看所有 XAML 控制項。
- [XAML 內容功能表範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>相關文章

- [MenuFlyout 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)
- [功能表列類別](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)
