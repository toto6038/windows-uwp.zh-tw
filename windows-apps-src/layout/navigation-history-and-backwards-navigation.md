---
author: mijacobs
Description: "在通用 Windows 平台 (UWP) app 中的瀏覽是以瀏覽結構、 瀏覽元素和系統層級功能的彈性模型為基礎。"
title: "通用 Windows 平台 (UWP) app 的瀏覽設計基本知識"
ms.assetid: e9876b4c-242d-402d-a8ef-3487398ed9b3
isNew: true
label: History and backwards navigation
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: a35b76f04d450aeafcc50c307dc058c52f6aebe4

---

#  瀏覽歷程記錄和向後瀏覽

在網際網路上，每個網站都提供自己的瀏覽系統，例如內容表格、按鈕、功能表、簡單的連結清單等。 不同網站之間的瀏覽體驗可能會有很大的差異。 不過，它們都有一項一致的瀏覽體驗：返回。 無論是何種網站，大部分瀏覽器都提供行為相同的返回按鈕。

基於相同理由，通用 Windows 平台 (UWP) 提供一致的返回瀏覽系統，以周遊使用者在應用程式內及應用程式之間 (視裝置而定) 的瀏覽歷程記錄。

系統返回按鈕的 UI 適合每種尺寸和輸入裝置類型，但每個裝置和 UWP 應用程式的瀏覽體驗卻是全域且一致的。

以下是返回按鈕 UI 的主要尺寸規格：


<table>
    <tr>
        <td colspan="2">裝置</td>
        <td>\[上一頁\] 按鈕行為</td>
     </tr>
    <tr>
        <td>手機</td>
        <td>![system back on a phone](images/back-systemback-phone.png)</td>
        <td>
        <ul>
<li>一律顯示。</li>
<li>裝置底部的軟體或硬體按鈕。</li>
<li>在 app 內和 app 間提供全域返回瀏覽。</li>
</ul>
</td>
     </tr>
     <tr>
        <td>平板電腦</td>
        <td>![system back on a tablet (in tablet mode)](images/back-systemback-tablet.png)</td>
        <td>
<ul>
<li>在平板電腦模式中一律顯示。

    Not available in Desktop mode. Title bar back button can be enabled, instead. See [PC, Laptop, Tablet](#PC).

    Users can switch between running in Tablet mode and Desktop mode by going to **Settings &gt; System &gt; Tablet mode** and setting **Make Windows more touch-friendly when using your device as a tablet**.</li>

<li> 裝置底部瀏覽列中的軟體按鈕。</li>
<li>在 app 內和 app 間提供全域返回瀏覽。</li></ul>        
        </td>
     </tr>
    <tr>
        <td>電腦、膝上型電腦、平板電腦</td>
        <td>![system back on a pc or laptop](images/back-systemback-pc.png)</td>
        <td>
<ul>
<li>在桌面模式中為選擇性。

    Not available in Tablet mode. See [Tablet](#Tablet).

    Disabled by default. Must opt in to enable it.

    Users can switch between running in Tablet mode and Desktop mode by going to **Settings &gt; System &gt; Tablet mode** and setting **Make Windows more touch-friendly when using your device as a tablet**.</li>

<li>App 標題列中的軟體按鈕。</li>
<li>只在 app 內提供返回瀏覽。 不支援 app 間瀏覽。</li></ul>        
        </td>
     </tr>
    <tr>
        <td>Surface Hub</td>
        <td>![system back on a surface hub](images/nav/nav-back-surfacehub.png)</td>
        <td>
<ul>
<li>選用。</li>
<li>預設為停用。 必須選擇加入才能啟用。</li>
<li>App 標題列中的軟體按鈕。</li>
<li>只在 app 內提供返回瀏覽。 不支援 app 間瀏覽。</li></ul>        
        </td>
     </tr>     
<table>


以下是一些替代的輸入類型，它們不依賴返回按鈕 UI，但仍提供相同的功能。


<table>
<tr><td colspan="3">輸入裝置</td></tr>
<tr><td>鍵盤</td><td>![keyboard](images/keyboard-wireframe.png)</td><td>Windows 鍵 + 退格鍵</td></tr>
<tr><td>Cortana</td><td>![speech](images/speech-wireframe.png)</td><td>請說「嗨 Cortana 返回」</td></tr>
</table>
 

當您的 app 在手機、平板電腦或啟用系統返回的電腦或膝上型電腦上執行時，按下返回按鈕後，系統就會通知您的 app。 使用者預期返回按鈕會瀏覽到應用程式瀏覽歷程的前一個位置。 您可以決定加入瀏覽歷程的瀏覽動作，以及如何回應按下返回按鈕。


## <span id="Enable_system_back_navigation_support"></span><span id="enable_system_back_navigation_support"></span><span id="ENABLE_SYSTEM_BACK_NAVIGATION_SUPPORT"></span>如何啟用系統返回瀏覽支援


App 必須啟用所有的硬體和軟體系統返回按鈕的返回瀏覽。 執行方式是登錄 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件的接聽程式並定義對應的處理常式。

我們現在在 App.xaml 程式碼後置檔案中登錄 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件的全域接聽程式。 如果某些頁面不要有返回瀏覽，或者您想要在顯示頁面之前執行頁面層級的程式碼，可以在每個頁面中針對此事件登錄。

```CSharp
Windows::UI::Core::SystemNavigationManager::GetForCurrentView()->
    BackRequested += ref new Windows::Foundation::EventHandler<
    Windows::UI::Core::BackRequestedEventArgs^>(
        this, &amp;App::App_BackRequested);
```

```CSharp
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += 
    App_BackRequested;
```

以下是對應的 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件處理常式，在 app 根框架上呼叫 [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568)。

這個處理常式會在發生全域返回事件時叫用。 如果 app 內的返回堆疊是空的，系統可能會瀏覽到 app 堆疊中的前一個 app 或 \[開始\] 畫面。 桌面模式沒有 app 返回堆疊，即使 app 內的返回堆疊用盡，使用者還是會留在 app 中。

```CSharp
void App::App_BackRequested(
    Platform::Object^ sender, 
    Windows::UI::Core::BackRequestedEventArgs^ e)
{
    Frame^ rootFrame = dynamic_cast<Frame^>(Window::Current->Content);
    if (rootFrame == nullptr)
        return;

    // Navigate back if possible, and if the event has not
    // already been handled.
    if (rootFrame->CanGoBack &amp;&amp; e->Handled == false)
    {
        e->Handled = true;
        rootFrame->GoBack();
    }
}
```

```CSharp
private void App_BackRequested(object sender, 
    Windows.UI.Core.BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
        return;

    // Navigate back if possible, and if the event has not 
    // already been handled .
    if (rootFrame.CanGoBack &amp;&amp; e.Handled == false)
    {
        e.Handled = true;
        rootFrame.GoBack();
    }
}
```
## <span id="Enable_the_title_bar_back_button"></span><span id="enable_the_title_bar_back_button"></span><span id="ENABLE_THE_TITLE_BAR_BACK_BUTTON"></span>如何啟用標題列返回按鈕


支援桌面模式 (通常是電腦和膝上型電腦，但有些平板電腦也能) 和已啟用設定 (\[設定\]  \[系統\]  \[平板電腦模式\]) 的裝置，不會同時提供全域瀏覽列和系統返回按鈕。

在桌面模式中，每個 app 都是在有標題列的視窗中執行。 您可以為 app 提供一個替代的返回按鈕，顯示在此標題列中。

只有當 app 在處於桌面模式的裝置上執行時，才會有標題列的返回按鈕，而且只支援 app 內瀏覽歷程記錄 — 不支援 app 間瀏覽歷程記錄。

**重要：**標題列的返回按鈕預設不會顯示。 您必須選擇加入。

 

|                                                             |                                                        |
|-------------------------------------------------------------|--------------------------------------------------------|
| ![桌面模式沒有系統返回](images/nav-noback-pc.png) | ![桌面模式的系統返回](images/nav-back-pc.png) |
| 桌面模式，沒有返回瀏覽。                           | 桌面模式，已啟用返回瀏覽。                 |

 

覆寫 [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 事件，並且為您想要啟用標題列返回按鈕的每個頁面，在程式碼後置檔案中將 [**AppViewBackButtonVisibility**](https://msdn.microsoft.com/library/windows/apps/dn986448) 設定為 [**Visible**](https://msdn.microsoft.com/library/windows/apps/dn986276)。

針對這個範例，如果框架之 [**CanGoBack**](https://msdn.microsoft.com/library/windows/apps/br242685) 屬性的值為 **true**，我們就列出返回堆疊中的每個頁面，然後啟用返回按鈕。

```ManagedCPlusPlus
void StartPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    auto rootFrame = dynamic_cast<Windows::UI::Xaml::Controls::Frame^>(Window::Current->Content);

    Platform::String^ myPages = "";

    if (rootFrame == nullptr)
        return;

    for each (PageStackEntry^ page in rootFrame->BackStack)
    {
        myPages += page->SourcePageType.ToString() + "\n";
    }
    stackCount->Text = myPages;

    if (rootFrame->CanGoBack)
    {
        // If we have pages in our in-app backstack and have opted in to showing back, do so
        Windows::UI::Core::SystemNavigationManager::GetForCurrentView()->AppViewBackButtonVisibility =
            Windows::UI::Core::AppViewBackButtonVisibility::Visible;
    }
    else
    {
        // Remove the UI from the title bar if there are no pages in our in-app back stack
        Windows::UI::Core::SystemNavigationManager::GetForCurrentView()->AppViewBackButtonVisibility =
            Windows::UI::Core::AppViewBackButtonVisibility::Collapsed;
    }
}
```

```CSharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    string myPages = "";
    foreach (PageStackEntry page in rootFrame.BackStack)
    {
        myPages += page.SourcePageType.ToString() + "\n";
    }
    stackCount.Text = myPages;

    if (rootFrame.CanGoBack)
    {
        // Show UI in title bar if opted-in and in-app backstack is not empty.
        SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
            AppViewBackButtonVisibility.Visible;
    }
    else
    {
        // Remove the UI from the title bar if in-app back stack is empty.
        SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
            AppViewBackButtonVisibility.Collapsed;
    }
}
```

### 自訂返回瀏覽行為的指導方針

如果您選擇提供自己的上一頁堆疊瀏覽，應該與其他 app 維持一致的使用體驗。 我們建議您遵循下列瀏覽動作模式：

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">瀏覽動作</th>
<th align="left">要新增至瀏覽歷程記錄嗎？</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>頁面之間、不同的對等群組</strong></p></td>
<td align="left"><strong>是</strong>
<p>在此圖例中，使用者從 app 的層級 1，跨對等群組瀏覽到層級 2，因此該瀏覽會新增至瀏覽歷程記錄。</p>
<p><img src="images/nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>在下一個圖例中，使用者在相同層級的兩個對等群組之間瀏覽，同樣是跨對等群組，所以此瀏覽會新增至瀏覽歷程記錄。</p>
<p><img src="images/nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>頁面之間、相同對等群組、沒有螢幕上的瀏覽元素</strong></p>
<p>使用者從相同對等群組內的一個頁面瀏覽到另一個頁面。 沒有提供直接瀏覽到兩頁面，且一律顯示的瀏覽元素 (如索引標籤/樞紐分析或停駐的瀏覽窗格)。</p></td>
<td align="left"><strong>是</strong>
<p>在以下圖例中，使用者在相同對等群組內的兩個頁面之間瀏覽。 頁面沒有使用索引標籤或停駐的瀏覽窗格，因此該瀏覽會新增至瀏覽歷程記錄。</p>
<p><img src="images/nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>頁面之間、相同對等群組、有螢幕上的瀏覽元素</strong></p>
<p>使用者從相同對等群組中的一個頁面瀏覽到另一個頁面。 兩個頁面都顯示在同一個瀏覽元素中。 例如，這兩個頁面使用相同的索引標籤/樞紐分析項目，或兩個頁面都顯示在停駐的瀏覽窗格中。</p></td>
<td align="left"><strong>否</strong>
<p>當使用者按下返回時，將返回使用者瀏覽到目前對等群組之前的最後一個頁面。</p>
<p><img src="images/nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td align="left"><strong>顯示暫時性 UI</strong>
<p>應用程式顯示快顯或子視窗 (例如對話方塊)、啟動畫面、螢幕小鍵盤，或應用程式進入特殊模式 (例如多重選取模式)。</p></td>
<td align="left"><strong>否</strong>
<p>當使用者按下返回按鈕時，關閉暫時性 UI (隱藏螢幕小鍵盤、取消對話方塊等等) 並返回產生暫時性 UI 的頁面。</p>
<p><img src="images/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td align="left"><strong>列舉項目</strong>
<p>App 會顯示螢幕項目的內容，例如主要/詳細資料清單中所選項目的詳細資料。</p></td>
<td align="left"><strong>否</strong>
<p>列舉項目類似於在對等群組中瀏覽。 當使用者按下返回時，會返回目前頁面之前有項目列舉的頁面。</p>
<img src="images/nav/nav-enumerate.png" alt="Iterm enumeration" /></td>
</tr>
</tbody>
</table>


### <span id="Resuming"></span><span id="resuming"></span><span id="RESUMING"></span>繼續

當使用者切換到其他 app，再回到您的 app 時，我們建議回到瀏覽歷程中的最後一個頁面。






 







<!--HONumber=Jun16_HO4-->


