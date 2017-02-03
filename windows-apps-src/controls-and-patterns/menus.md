---
author: mijacobs
Description: "飛出視窗是輕量型的快顯視窗，用來暫時顯示與使用者目前正在執行的操作相關的 UI。"
title: "功能表和操作功能表"
label: Menus and context menus
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 5f50e490caa5d1d88c2f8315dc47e15b0ae22a05
ms.openlocfilehash: badb03c97ae0f2350e5d7592f10168bb7d6e7d1a

---
# <a name="menus-and-context-menus"></a>功能表和操作功能表

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

功能表和操作功能表會在使用者要求命令或選項時，顯示它們的清單。

![一般操作功能表的範例](images/controls_contextmenu_singlepane.png)

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)</li>
<li>[ContextFlyout 屬性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx)</li>
<li>[FlyoutBase.AttachedFlyout 屬性](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx)</li>
</ul>
</div>


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？
功能表和操作功能表可組織命令並在使用者不需要它們時加以隱藏，藉以節省空間。 如果經常會用到某個特定命令，而且您有可用的空間，請考慮直接將它放置於它自己的元素中，而不是放在功能表中，讓使用者不需瀏覽功能表，即可取得該命令。 

功能表和操作功能表適用於組織命令。若要顯示任意內容 (例如通知或要求確認)，請使用[對話方塊或飛出視窗](dialogs.md)。  


## <a name="menus-vs-context-menus"></a>功能表與操作功能表

功能表和操作功能表在其外觀和可包含的內容部分是完全相同的。 事實上，您會使用同一個控制項 [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) 來建立它們。 唯一的差別在於使用者存取它的方式。 

您何時應該使用功能表或操作功能表？
* 如果裝載元素為按鈕或一些其他命令元素，且其主要作用是呈現其他命令，請使用功能表。
* 如果裝載元素是一些具有不同主要用途 (例如呈現文字或影像) 的其他類型元素，請使用操作功能表。 

例如，在瀏覽窗格的按鈕上使用功能表，來提供額外的瀏覽選項。 在這個案例中，按鈕控制項的主要用途是提供功能表的存取。 

如果您想要在文字元素中新增命令 (例如剪下、複製及貼上)，請使用操作功能表，而不是功能表。 在這個案例中，文字元素的主要作用是呈現和編輯文字；其他命令 (例如剪下、複製及貼上) 均為次要的且隸屬於操作功能表。 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>功能表</b></p>
<p>
<ul>
<li>具有一律顯示的單一進入點 (例如，位於畫面頂端的 [檔案] 功能表)。</li>
<li>通常會附加到按鈕或父功能表項目。</li>
<li>是透過按一下滑鼠左鍵 (或對等的動作，例如使用手指點選) 來叫用。</li>  
<li>透過 [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) 或 [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) 屬性來與元素產生關聯。</li> 
</ul>
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>操作功能表</b></p>
   
<ul>
<li>會附加到單一元素，但只有當內容有意義時才可存取。</li>
<li>是透過按一下滑鼠右鍵 (或對等的動作，例如使用您的手指長按) 來叫用。</li>
<li>透過其 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 屬性來與元素產生關聯。  
</ul><br/>

  </div>
</div>
</div>

## <a name="create-a-menu-or-a-context-menu"></a>建立功能表或操作功能表

若要建立功能表或操作功能表，您可以使用 [MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)。 您可以藉由將 [MenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx)、[ToggleMenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 及 [MenuFlyoutSeparator](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) 物件新增到 MenuFlyout，來定義功能表的內容。 這些物件的用途如下：
* [MenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx) - 執行立即的動作。
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) - 開啟或關閉選項。
* [MenuFlyoutSeparator](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) - 在視覺上分隔功能表項目。


這個範例會建立 [MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)，並使用 [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 屬性 (適用於大多數控制項的屬性)，以將 [MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)顯示為操作功能表。

````xaml
<Rectangle 
  Height="100" Width="100" 
  Tapped="Rectangle_Tapped">
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

下一個範例幾乎完全相同，但不會使用 [ContextFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) 屬性來將 [MenuFlyout 類別](https://msdn.microsoft.com/library/windows/apps/dn299030)顯示為操作功能表，這個範例改用 [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 屬性來將它顯示為功能表。 

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


> 消失關閉控制項 (例如，功能表、操作功能表及其他飛出視窗) 會將鍵盤和遊戲台焦點困在暫時性 UI 內，直到關閉為止。 若要提供此行為的視覺提示，Xbox 上的消失關閉控制項將會繪製重疊，以使超出範圍 UI 的可見度變暗。 您可以使用新的 [LightDismissOverlayMode](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx) 屬性來修改此行為。 根據預設，暫時性 UI 將在 Xbox 上繪製消失關閉重疊，但不會在其他裝置系列上繪製，不過 app 可以選擇將重疊強制為一律**開啟**或一律**關閉**。

> ```xaml
> <MenuFlyout LightDismissOverlayMode=\"Off\">
> ```

## <a name="get-the-sample-code"></a>取得範例程式碼
*   [XAML UI 基本知識範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [**MenuFlyout 類別**](https://msdn.microsoft.com/library/windows/apps/dn299030)



<!--HONumber=Dec16_HO2-->


