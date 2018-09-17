---
author: mijacobs
Description: Menus and context menus display a list of commands or options when the user requests them.
title: 功能表和操作功能表
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 07/19/2018
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
ms.openlocfilehash: 703667bf22ce11c119463008e868a943d447c7ff
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2018
ms.locfileid: "3986309"
---
# <a name="menus-and-context-menus"></a>功能表和操作功能表

> [!IMPORTANT]
> 本文說明尚未發佈但可能在正式發行前大幅度修改的功能。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。 預覽功能，需要的[最新的 Windows 10 Insider Preview 組建和 SDK](https://insider.windows.com/for-developers/) ] 或 [ [Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

功能表和操作功能表會在使用者要求命令或選項時，顯示它們的清單。 您可以使用功能表飛出視窗來顯示單一，內嵌功能表。 您可以使用功能表列在水平列中，通常在應用程式視窗的頂端顯示一組的功能表。 每個功能表可以有功能表項目和子功能表。

![一般操作功能表的範例](images/contextmenu_rs2_icons.png)

| **取得 Windows UI 文件庫** |
| - |
| 此控制項是包含在 Windows UI 程式庫，包含新的控制項和 UI 功能適用於 UWP app 的 NuGet 套件。 如需詳細資訊，包括安裝指示，請參閱[Windows UI 文件庫的概觀](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

| **平台 Api** | **Windows 使用者介面程式庫 Api** |
| - | - |
| [MenuFlyout 類別](/uwp/api/windows.ui.xaml.controls.menuflyout)，[功能表列的類別](/uwp/api/windows.ui.xaml.controls.menubar)， [ContextFlyout 屬性](/uwp/api/windows.ui.xaml.uielement.contextflyout)， [FlyoutBase.AttachedFlyout 屬性](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout) | [功能表列類別](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

功能表和操作功能表可組織命令並在使用者不需要它們時加以隱藏，藉以節省空間。 如果經常會用到某個特定命令，而且您有可用的空間，請考慮直接將它放置於它自己的元素中，而不是放在功能表中，讓使用者不需瀏覽功能表，即可取得該命令。

功能表和操作功能表是適用於組織命令;若要顯示任意內容，例如通知或確認要求，使用[對話方塊或飛出視窗](dialogs.md)。

### <a name="menubar-vs-menuflyout"></a>功能表列與 MenuFlyout

若要附加到畫布上 UI 元素的飛出視窗中顯示在功能表中，使用 MenuFlyout 控制項來裝載您的功能表項目。 您可以叫用功能表飛出視窗為一般的功能表或操作功能表。 功能表飛出視窗會裝載單一的最上層功能表 （和選擇性的子功能表）。

若要在水平列中顯示一組的多個最上層的功能表，請使用功能表列。 您通常會放置在應用程式視窗的頂端的功能表列。

### <a name="menubar-vs-commandbar"></a>功能表列與 CommandBar

功能表列和 CommandBar 兩者代表您可用來公開命令給您的使用者的表面。 功能表列提供快速且簡易的方式可以公開一組命令可能需要更多的組織或群組的應用程式，然後 CommandBar 允許。

您也可以使用功能表列與 CommandBar 搭配使用。 您可以使用功能表列來提供大量的命令，以及 CommandBar 反白顯示的最常用的命令。

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

功能表和操作功能表會在其外觀和可包含類似。 事實上，您可以使用相同的控制項， [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)，來建立它們。 不同之處在於讓使用者存取它的方式。

您何時應該使用功能表或操作功能表？

- 如果裝載元素為按鈕或一些其他命令元素，且其主要作用是呈現其他命令，請使用功能表。
- 如果裝載元素是一些具有不同主要用途 (例如呈現文字或影像) 的其他類型元素，請使用操作功能表。

提供篩選與排序清單的選項，例如，在按鈕上使用功能表。 在這個案例中，按鈕控制項的主要用途是提供功能表的存取。

![郵件中功能表的範例](images/Mail_Menu.png)

如果您想要在文字元素中新增命令 (例如剪下、複製及貼上)，請使用操作功能表，而不是功能表。 在這個案例中，文字元素的主要作用是呈現和編輯文字；其他命令 (例如剪下、複製及貼上) 均為次要的且隸屬於操作功能表。

![影像中心的操作功能表範例](images/ContextMenu_example.png)

### <a name="menus"></a>功能表

- 具有一律顯示的單一進入點 (例如，位於畫面頂端的 [檔案] 功能表)。
- 通常會附加到按鈕或父功能表項目。
- 是透過按一下滑鼠左鍵 (或對等的動作，例如使用手指點選) 來叫用。
- [飛出視窗](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx)或[FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx)屬性，透過元素相關聯或分組在功能表列在應用程式視窗的頂端。

### <a name="context-menus"></a>操作功能表

- 已連結至單一元素，並會顯示次要命令。
- 是透過按一下滑鼠右鍵 (或對等的動作，例如使用您的手指長按) 來叫用。
- 透過其 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 屬性來與元素產生關聯。

## <a name="icons"></a>圖示

請考慮將功能表項目圖示提供給：

- 最常使用的項目。
- 圖示為標準或已知眾所周知的功能表項目。
- 圖示清楚表明命令會執行的功能表項目。

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
> MenuFlyoutItem 中圖示的大小為 16x16px。 如果您使用 SymbolIcon、 FontIcon 或 PathIcon，圖示會自動調整以正確大小逼真度不會遺失。 如果使用 BitmapIcon，請確定您的資產為 16x16px。  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>建立功能表飛出視窗或操作功能表

若要建立功能表飛出視窗或操作功能表，您可以使用[MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)。 您可以藉由將 [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx)、[ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 及 [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) 物件新增到 MenuFlyout，來定義功能表的內容。

這些物件的用途如下：

- [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx) - 執行立即的動作。
- [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) - 開啟或關閉選項。
- [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) - 在視覺上分隔功能表項目。

這個範例會建立[MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) ，並使用[ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx)屬性，適用於大多數控制項，屬性顯示為操作功能表 MenuFlyout。

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

### <a name="light-dismiss"></a>消失關閉

消失關閉控制項 (例如，功能表、操作功能表及其他飛出視窗) 會將鍵盤和遊戲台焦點困在暫時性 UI 內，直到關閉為止。 若要提供此行為的視覺提示，Xbox 上的消失關閉控制項將會繪製重疊，以使超出範圍 UI 的可見度變暗。 您可以使用新的 [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx) 屬性來修改此行為。 根據預設，暫時性 UI 將在 Xbox (**\[自動\]**) 上繪製消失關閉重疊，但不會在其他裝置系列上繪製，不過應用程式可以選擇強制重疊一律為 **\[開啟\]** 或一律為 **\[關閉\]**。

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>建立功能表列

> **預覽**： 功能表列需要的[最新的 Windows 10 Insider Preview 組建和 SDK](https://insider.windows.com/for-developers/) ] 或 [ [Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

您可以使用相同的項目來建立如所示的功能表飛出視窗功能表列中的功能表。 不過，而不是群組中的 MenuFlyout MenuFlyoutItem 物件，您將它們分組 MenuBarItem 項目中。 每個 MenuBarItem 新增到功能表列上，為最上層功能表。

![功能表列的範例](images/menu-bar-submenu.png)

> [!NOTE]
> 這個範例示範如何只建立 UI 結構，但不是顯示任何命令的實作。

```xaml
<MenuBar>
    <MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save">
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </MenuBarItem>

    <MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </MenuBarItem>

    <MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </MenuBarItem>
</MenuBar>
```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)：以互動式格式查看所有 XAML 控制項。
- [XAML 操作功能表範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>相關文章

- [MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [功能表列類別](/uwp/api/Windows.UI.Xaml.Controls.MenuBar)
