---
author: serenaz
Description: The Universal Windows Platform (UWP) provides a consistent back navigation system for traversing the user's navigation history within an app and, depending on the device, from app to app.
title: 瀏覽歷程記錄和向後瀏覽 (Windows 應用程式)
ms.assetid: e9876b4c-242d-402d-a8ef-3487398ed9b3
isNew: true
label: History and backwards navigation
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/22/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 824f0e83408893bf95d856067282b1fea1313876
ms.sourcegitcommit: 588171ea8cb629d2dd6aa2080e742dc8ce8584e5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2018
ms.locfileid: "1895405"
---
#  <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>適用於 UWP app 的瀏覽歷程記錄和向後瀏覽

> **重要 API**：[BackRequested 事件](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested)、[SystemNavigationManager 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager)、[OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

通用 Windows 平台 (UWP) 提供一致的返回瀏覽系統，以周遊使用者在應用程式內及應用程式之間 (視裝置而定) 的瀏覽歷程記錄。

若要在您的 app 中實作向後瀏覽，請將[返回按鈕](#Back-button)放在 App UI 的左上角。 如果您的 app 使用 [NavigationView](../controls-and-patterns/navigationview.md) 控制項，則您可以使用 [NavigationView 的內建返回按鈕](../controls-and-patterns/navigationview.md#backwards-navigation)。 

使用者預期返回按鈕會瀏覽到應用程式瀏覽歷程的前一個位置。 請注意，您可以決定加入瀏覽歷程的瀏覽動作，以及如何回應按下返回按鈕。

## <a name="back-button"></a>返回按鈕

若要建立返回按鈕，請使用 [Button](../controls-and-patterns/buttons.md) 控制項搭配 `NavigationBackButtonNormalStyle` 樣式，並將按鈕放在 App UI 的左上角。

![App UI 左上角的返回按鈕](images/back-nav/BackEnabled.png)

```xaml
<Button Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

如果您的 App 有頂端 [CommandBar](../controls-and-patterns/app-bars.md)，則高度 44px 的 Button 控制項將不會精準對齊 48px AppBarButtons。 不過，為避免不一致，請在 48px 界限內部對齊 Button 控制項的頂端。

![頂端命令列上的返回按鈕](images/back-nav/CommandBar.png)

```xaml
<Button VerticalAlignment="Top" HorizontalAlignment="Left" 
Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

為了將 UI 元素最小化，以便在 App 中四處移動，請在上一頁堆疊中沒有內容時顯示已停用的返回按鈕 (請參閱下方程式碼範例)。

![返回按鈕狀態](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>程式碼範例

下列程式碼範例示範如何使用返回按鈕實作向後瀏覽行為。 程式碼對應至 Button [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) 事件，並在瀏覽至新頁面時呼叫的 [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_) 中停用/啟用按鈕可見度。 程式碼範例也會處理來自硬體和軟體系統返回按鍵的輸入，方法是登錄 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) 事件的接聽程式。

```xaml
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

程式碼後置：

```csharp
public MainPage()
{
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    BackButton.IsEnabled = this.Frame.CanGoBack;
}

private void Back_Click(object sender, RoutedEventArgs e)
{
    On_BackRequested();
}

// Handles system-level BackRequested events and page-level back button Click events
private bool On_BackRequested()
{
    if (this.Frame.CanGoBack)
    {
        this.Frame.GoBack();
        return true;
    }
    return false;
}

private void BackInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}
```

我們現在於 `App.xaml` 程式碼後置檔案中登錄 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) 事件的全域接聽程式。 如果某些頁面不要有返回瀏覽，或者您想要在顯示頁面之前執行頁面層級的程式碼，可以在每個頁面中針對此事件登錄。

App.xaml 程式碼後置：

```csharp
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed = e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
}
```

## <a name="optimizing-for-different-device-and-form-factors"></a>針對不同的裝置與外形規格最佳化

此向後瀏覽設計指導方針適用於所有裝置，但不同的裝置和外形規格可能因最佳化而受惠。 這也取決於不同殼層支援的硬體返回按鈕。

- **手機/平板電腦**：硬體或軟體返回按鈕一律顯示在行動電話和平板電腦上，但我們建議另設應用程式內返回按鈕以便清楚說明。
- **桌面/中心**：在 App UI 左上角設置應用程式內返回按鈕。
- **Xbox/電視**：不設置返回按鈕，因為這樣會增添不必要的 UI 混雜度。 改用遊戲台 B 按鈕向後瀏覽。

如果您的 App 將在多個裝置上執行，[為 Xbox 建立自訂的視覺化觸發程序](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox)以切換按鈕的可見性。 如果您的 app 在 Xbox 上執行，NavigationView 控制項會自動切換返回按鈕的可見性。 

我們建議提供下列輸入的支援以便返回瀏覽。 (請注意，當中某些輸入不受系統 BackRequested 支援，必須另以不同的事件處理。)

| 輸入 | 事件 |
| --- | --- |
| Windows 退格鍵 | BackRequested |
| 硬體返回按鈕 | BackRequested |
| 殼層平板電腦模式返回按鈕 | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| Alt+向左鍵 | KeyboardAccelerator.BackInvoked |

上面提供的程式碼範例示範如何處理這些輸入。

## <a name="system-back-behavior-for-backward-compatibilities"></a>提供回溯相容性的系統返回行為

之前 UWP app 是使用 [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility) 提供返回瀏覽。 API 將繼續受到支援以提供回溯相容性，但我們不再建議依賴標題列返回按鈕。 您的 App 應設置自己的應用程式內返回按鈕。

如果您的 app 繼續使用 [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility)，則返回按鈕會如往常出現在標題列內。

![標題列返回按鈕](images/nav-back-pc.png)

## <a name="guidelines-for-custom-back-navigation-behavior"></a>自訂返回瀏覽行為的指導方針

如果您選擇提供自己的上一頁堆疊瀏覽，應該與其他 app 維持一致的使用體驗。 我們建議您遵循下列瀏覽動作模式：

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">瀏覽動作</th>
<th align="left">要新增至瀏覽歷程記錄嗎？</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>頁面之間、不同的對等群組</strong></td>
<td style="vertical-align:top;"><strong>是</strong>
<p>在此圖例中，使用者從 app 的層級 1，跨對等群組瀏覽到層級 2，因此該瀏覽會新增至瀏覽歷程記錄。</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>在下一個圖例中，使用者在相同層級的兩個對等群組之間瀏覽，同樣是跨對等群組，所以此瀏覽會新增至瀏覽歷程記錄。</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>頁面之間、相同對等群組、沒有螢幕上的瀏覽元素</strong>
<p>使用者從相同對等群組內的一個頁面瀏覽到另一個頁面。 沒有提供直接瀏覽到兩頁面，且一律顯示的瀏覽元素 (如上方瀏覽窗格或停駐的左瀏覽窗格)。</p></td>
<td style="vertical-align:top;"><strong>是</strong>
<p>在以下圖例中，使用者在相同對等群組內的兩個頁面之間瀏覽。 頁面沒有使用上方瀏覽列或停駐的左瀏覽窗格，因此該瀏覽會新增至瀏覽歷程記錄。</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>頁面之間、相同對等群組、有螢幕上的瀏覽元素</strong>
<p>使用者從相同對等群組中的一個頁面瀏覽到另一個頁面。 兩個頁面都顯示在同一個瀏覽元素中。 例如，這兩個頁面使用相同的上方窗格元素，或兩個頁面都顯示在停駐的左瀏覽窗格中。</p></td>
<td style="vertical-align:top;"><strong>不一定</strong>
<p>是的，新增至瀏覽歷程記錄，但有 2 個值得注意的例外。 如果您希望您的 App 使用者頻繁在對等群組中的頁面間切換，或者如果您想要保留對等群組的頁面中的瀏覽狀態/歷程記錄，則請勿新增到瀏覽歷程記錄。 在此情況下，當使用者按下返回時，將返回使用者瀏覽到目前對等群組之前的最後一個頁面。 </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>顯示暫時性 UI</strong>
<p>應用程式顯示快顯或子視窗 (例如對話方塊)、啟動畫面、螢幕小鍵盤，或應用程式進入特殊模式 (例如多重選取模式)。</p></td>
<td style="vertical-align:top;"><strong>否</strong>
<p>當使用者按下返回按鈕時，關閉暫時性 UI (隱藏螢幕小鍵盤、取消對話方塊等等) 並返回產生暫時性 UI 的頁面。</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>列舉項目</strong>
<p>App 會顯示螢幕項目的內容，例如主要/詳細資料清單中所選項目的詳細資料。</p></td>
<td style="vertical-align:top;"><strong>否</strong>
<p>列舉項目類似於在對等群組中瀏覽。 當使用者按下返回時，會返回目前頁面之前有項目列舉的頁面。</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>繼續

當使用者切換到其他 app，再回到您的 app 時，我們建議回到瀏覽歷程記錄中的最後一個頁面

## <a name="related-articles"></a>相關文章

- [瀏覽基本知識](navigation-basics.md)