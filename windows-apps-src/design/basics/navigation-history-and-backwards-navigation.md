---
Description: 了解如何實作向後導覽周遊 UWP 應用程式中的使用者的瀏覽歷程記錄。
title: 瀏覽歷程記錄和向後瀏覽 (Windows 應用程式)
template: detail.hbs
op-migration-status: ready
ms.date: 06/21/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c74d4ebd08dfeddfb4a0149cffcd7bb845ceff11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595043"
---
# <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>適用於 UWP app 的瀏覽歷程記錄和向後瀏覽

> **重要的 Api**:[BackRequested 事件](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested)， [SystemNavigationManager 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager)， [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

通用 Windows 平台 (UWP) 提供一致的返回瀏覽系統，以周遊使用者在應用程式內及應用程式之間 (視裝置而定) 的瀏覽歷程記錄。

若要在您的 app 中實作向後瀏覽，請將[返回按鈕](#Back-button)放在 App UI 的左上角。 如果您的 app 使用 [NavigationView](../controls-and-patterns/navigationview.md) 控制項，則您可以使用 [NavigationView 的內建返回按鈕](../controls-and-patterns/navigationview.md#backwards-navigation)。

使用者預期返回按鈕會瀏覽到應用程式瀏覽歷程的前一個位置。 請注意，您可以決定加入瀏覽歷程的瀏覽動作，以及如何回應按下返回按鈕。

## <a name="back-button"></a>返回按鈕

若要建立 [上一頁] 按鈕，使用[ 按鈕](../controls-and-patterns/buttons.md)控制項取代`NavigationBackButtonNormalStyle`樣式，並放置在您的應用程式 UI 的左上角的按鈕 （如需詳細資訊，請參閱下面的 XAML 程式碼範例）。

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

為了將 UI 元素最小化，以便在 App 中四處移動，請在上一頁堆疊中沒有內容時顯示已停用的返回按鈕 (請參閱下方程式碼範例)。 不過，如果您預期您的應用程式絕對不會有 backstack，您不需要完全顯示 [上一頁] 按鈕。

![返回按鈕狀態](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>程式碼範例

下列程式碼範例示範如何使用返回按鈕實作向後瀏覽行為。 程式碼對應至 Button [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) 事件，並在瀏覽至新頁面時呼叫的 [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_) 中停用/啟用按鈕可見度。 程式碼範例也會處理來自硬體和軟體系統返回按鍵的輸入，方法是登錄 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) 事件的接聽程式。

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

程式碼後置：

```csharp
// MainPage.xaml.cs
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

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Navigation.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;

namespace winrt::PageNavTest::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();

        Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
        goBack.Key(Windows::System::VirtualKey::GoBack);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
        altLeft.Key(Windows::System::VirtualKey::Left);
        altLeft.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(goBack);
        KeyboardAccelerators().Append(altLeft);
        // ALT routes here.
        altLeft.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
    }

    void MainPage::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
    {
        BackButton().IsEnabled(Frame().CanGoBack());
    }

    void MainPage::Back_Click(IInspectable const&, RoutedEventArgs const&)
    {
        On_BackRequested();
    }

    // Handles system-level BackRequested events and page-level back button Click events.
    bool MainPage::On_BackRequested()
    {
        if (Frame().CanGoBack())
        {
            Frame().GoBack();
            return true;
        }
        return false;
    }

    void MainPage::BackInvoked(Windows::UI::Xaml::Input::KeyboardAccelerator const& sender,
        Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
    {
        args.Handled(On_BackRequested());
    }
}
```

以上所述，我們便接著處理回溯的單頁瀏覽。 如果您想要從向後巡覽，排除特定頁面，或您想要執行頁面層級程式碼之前顯示的頁面，您可以處理每個頁面中的導覽。

為了處理向後瀏覽整個應用程式，您會註冊通用的接聽程式，如[ **BackRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested)中的事件`App.xaml`程式碼後置檔案。

App.xaml 程式碼後置：

```csharp
// App.xaml.cs
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
}

private bool On_BackRequested()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}
```

```cppwinrt
// App.cpp
#include <winrt/Windows.UI.Core.h>
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"

#include "App.h"
#include "MainPage.h"

using namespace winrt;
...

    Windows::UI::Core::SystemNavigationManager::GetForCurrentView().BackRequested({ this, &App::App_BackRequested });
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }
    rootFrame.PointerPressed({ this, &App::On_PointerPressed });
...

void App::App_BackRequested(IInspectable const& /* sender */, Windows::UI::Core::BackRequestedEventArgs const& e)
{
    e.Handled(On_BackRequested());
}

void App::On_PointerPressed(IInspectable const& sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender.as<UIElement>()).Properties().PointerUpdateKind() == Windows::UI::Input::PointerUpdateKind::XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled(On_BackRequested());
    }
}

// Handles system-level BackRequested events.
bool App::On_BackRequested()
{
    if (Frame().CanGoBack())
    {
        Frame().GoBack();
        return true;
    }
    return false;
}
```

## <a name="optimizing-for-different-device-and-form-factors"></a>針對不同的裝置與外形規格最佳化

此向後瀏覽設計指導方針適用於所有裝置，但不同的裝置和外形規格可能因最佳化而受惠。 這也取決於不同殼層支援的硬體返回按鈕。

- **電話/Tablet**:硬體或軟體的返回按鈕一律是在行動裝置上呈現和平板電腦，但我們建議您將繪圖應用程式內上一頁按鈕，為了清楚起見。
- **桌面/中樞**:繪製應用程式內的返回按鈕，在您的應用程式 UI 的左上角。
- **Xbox/電視**:它會加入不必要的 UI 雜亂不繪製 [上一頁] 按鈕。 改用遊戲台 B 按鈕向後瀏覽。

如果您的 App 將在多個裝置上執行，[為 Xbox 建立自訂的視覺化觸發程序](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox)以切換按鈕的可見性。 如果您的 app 在 Xbox 上執行，NavigationView 控制項會自動切換返回按鈕的可見性。 

我們建議提供下列輸入的支援以便返回瀏覽。 (請注意，當中某些輸入不受系統 BackRequested 支援，必須另以不同的事件處理。)

| Input | 事件 |
| --- | --- |
| Windows 退格鍵 | BackRequested |
| 硬體返回按鈕 | BackRequested |
| 殼層平板電腦模式返回按鈕 | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| Alt+向左鍵 | KeyboardAccelerator.BackInvoked |

上面提供的程式碼範例示範如何處理這些輸入。

## <a name="system-back-behavior-for-backward-compatibilities"></a>提供回溯相容性的系統返回行為

之前 UWP app 是使用 [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility) 提供返回瀏覽。 API 會繼續受到支援，以確保回溯相容性，但我們不再建議仰賴[AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility)。 您的 App 應設置自己的應用程式內返回按鈕。

如果您的應用程式還是會繼續使用[AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility)，則系統 UI 會呈現系統上一頁按鈕的標題列內部。 （如 [上一頁] 按鈕的外觀和使用者的互動不會變更來自前一個組建。）

![標題列上一步按鈕](images/nav-back-pc.png)

### <a name="system-back-bar"></a>系統後列

> [!NOTE]
> 「 系統後列 」 是只描述，不的官方名稱。

系統後列是 「 矩形 」 插入 索引標籤上的頻外和應用程式的內容區域之間。 色調跨越了整個應用程式的寬度，與左邊緣的返回按鈕。 視覺帶具有垂直 32 像素為單位，以確保有足夠的觸控目標大小的 [上一頁] 按鈕的高度。

根據返回按鈕可見度，動態顯示系統背面列。 上一頁 按鈕時顯示，系統後插入列時，向下調整應用程式內容，x 32 像素 索引標籤群組列下方。 [上一頁] 按鈕會隱藏時，系統後列以動態方式移除後，調整應用程式內容，x 32 像素，以符合索引標籤群組列。 若要避免您的應用程式 UI shift 向上或向下，我們建議繪圖[應用程式內的返回按鈕](#back-button)。

[標題列自訂](../shell/title-bar.md)會延續到回系統和應用程式 索引標籤列。 如果您的應用程式指定具有背景和前景色彩屬性[ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar)，則色彩會套用至索引標籤和系統後列。

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
<td style="vertical-align:top;"><strong>頁面上，不同的對等群組的頁面</strong></td>
<td style="vertical-align:top;"><strong>[是]</strong>
<p>在此圖例中，使用者從 app 的層級 1，跨對等群組瀏覽到層級 2，因此該瀏覽會新增至瀏覽歷程記錄。</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>在下一個圖例中，使用者在相同層級的兩個對等群組之間瀏覽，同樣是跨對等群組，所以此瀏覽會新增至瀏覽歷程記錄。</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>頁面 頁面上，以相同對等互連 群組中，沒有螢幕上瀏覽項目</strong>
<p>使用者從相同對等群組內的一個頁面瀏覽到另一個頁面。 沒有螢幕上沒有瀏覽項目 (例如<a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>)，提供直接瀏覽至這兩個頁面。</p></td>
<td style="vertical-align:top;"><strong>[是]</strong>
<p>在下圖中，使用者在相同的對等群組中，兩個頁面之間巡覽，瀏覽應新增至巡覽記錄。</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>頁面，即可 頁面上，相同的對等群組與螢幕上瀏覽項目</strong>
<p>使用者從相同對等群組中的一個頁面瀏覽到另一個頁面。 這兩個頁面所示的相同的瀏覽項目，例如<a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>。</p></td>
<td style="vertical-align:top;"><strong>它相依</strong>
<p>是，新增至巡覽記錄中，有兩個值得注意的例外狀況。 如果您預期您的應用程式的使用者通常對等群組中的頁面之間切換，或者您想要保留的導覽階層，然後不要新增至巡覽記錄。 在此情況下，當使用者按下返回時，將返回使用者瀏覽到目前對等群組之前的最後一個頁面。 </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>顯示暫時性的 UI</strong>
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

當使用者切換到其他 app，再回到您的 app 時，我們建議回到瀏覽歷程記錄中的最後一個頁面。

## <a name="related-articles"></a>相關文章

- [瀏覽基本知識](navigation-basics.md)
