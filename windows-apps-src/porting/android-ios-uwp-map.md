---
title: 比較 iOS、Android 和 Windows 10 平台之間的功能。
description: 查看 Windows 10 上的 iOS、Android 和通用 Windows 平臺 (UWP) 之間的開發概念和平臺功能的詳細比較。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: d15f0b1dccbdb165cb2bbcf39628e16859a42ed5
ms.sourcegitcommit: c5fdcc0779d4b657669948a4eda32ca3ccc7889b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2021
ms.locfileid: "102784729"
---
# <a name="windows-apps-concept-mapping-for-android-and-ios-developers"></a>適用於 Android 與 iOS 開發人員的 Windows 應用程式概念對應

如果您是具備 Android 或 iOS 技巧和 (或) 程式碼的開發人員，而且您想要移到 Windows 10 和通用 Windows 平台 (UWP)，則此資源擁有您在三個平台之間對應平台功能 (和您的知識) 所需的資訊。

另請參閱[從 iOS 移到 UWP](ios-to-uwp-root.md) 中的移植內容。 這份文件也可供[下載](https://www.microsoft.com/download/details.aspx?id=52041)。

## <a name="user-interface-ui"></a>使用者介面 (UI)


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>設計語言。</strong><br><br>一組擬定平台上 app 外觀和行為的慣例。</td>
<td align="left"><strong>Android Material Design (Android 材料設計)</strong> 指導方針提供 Android 設計人員與開發人員需要遵循的視覺化語言。</td>
<td align="left"><strong>Human Interface Guidelines (人性化介面指導方針)</strong> 為 iOS 設計人員與開發人員提供建議。</td>
<td align="left"><a href="https://developer.microsoft.com/windows/apps/design"><strong>UWP Windows 應用程式設計</strong></a>示範如何建立在所有 Windows 10 裝置上具有出色外觀的 App。 您可以找到使用者介面 (UI) 設計基礎知識、回應式設計技術，以及詳細指導方針的完整清單。<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>使用者介面標記語言。</strong> <br><br>轉譯和描述 UI 及其元件的標記語言。 每個平台都為視覺與標記編輯提供編輯器。<br/></td>
<td align="left"><strong>XML 版面</strong>配置，使用 <strong>Android Studio</strong> 或 <strong>Eclipse</strong>進行編輯。</td>
<td align="left"><strong>XIB</strong> 和 <strong>Storyboard (腳本)</strong>，使用 Xcode 內的 <strong>Interface Builder (介面建立器)</strong> 編輯。</td>
<td align="left"><strong><a href="/windows/uwp/xaml-platform/xaml-overview">XAML</a></strong>，使用 <strong><a href="https://visualstudio.microsoft.com/">Microsoft Visual Studio</a></strong> 和 <strong><a href="/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend for Visual Studio</a></strong> 編輯。<br/><br/><a href="/windows/uwp/xaml-platform/index">XAML 平台</a><br/><br/><a href="/windows/uwp/design/basics/xaml-basics-ui">使用 XAML 建立 UI</a><br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">使用 XAML 定義版面配置</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>內建的使用者介面控制項。</strong> <br><br>平台提供的可重複使用的 UI 元素，例如按鈕、清單控制項與文字控制項。</td>
<td align="left">預先建置的 <strong>view (檢視)</strong> 和 <strong>view group (檢視群組)</strong> 類別，稱為小工具、版面配置、文字欄位、容器、日期/時間控制項與專家控制項。</td>
<td align="left"><strong>View (檢視)</strong> 和 <strong>Control (控制項)</strong> 可在 Xcode 物件程式庫中找到，並列示於 UIKit 使用者介面的類別目錄中。 View (檢視) 包含影像檢視、選擇器檢視和捲動檢視。 Controls (控制項) 包含按鈕、日期選擇器和文字欄位。</td>
<td align="left">XAML 平台提供一組豐富的<strong>內建控制項</strong>，例如按鈕、清單控制項、面板、文字控制項、命令列、選擇器、媒體與筆跡。<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">新增控制項和處理事件</a></td>
</tr>
<tr class="even">
<td align="left"><strong>控制事件處理。</strong> <br><br>定義觸發 UI 控制項內事件時要執行的邏輯。</td>
<td align="left"><strong>Event handlers (事件處理常式)</strong> 和 <strong>Event listeners (事件接聽程式)</strong> 以 XML 或以程式設計方式新增。</td>
<td align="left">控制項可將 <strong>action (動作)</strong> 訊息傳送到 <strong>targets (目標)</strong>。</td>
<td align="left">您可以定義方法來處理附加到 XAML 頁面之<strong>程式碼後置檔案</strong>中 XAML 控制項的事件。 <strong>事件處理常式</strong>一律以程式碼撰寫。 但是您可以使用 XAML 標記或程式碼將這些處理常式勾連到事件。<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">新增控制項和處理事件</a><br/><br/><a href="/windows/uwp/xaml-platform/events-and-routed-events-overview">事件與路由事件概觀</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>資料系結。</strong> <br><br>一種軟體設計模式，可讓 app UI 轉譯資料，而且可以選擇與該資料保持同步。</td>
<td align="left">目前已提供 <strong>Data Binding Library (資料繫結程式庫)</strong>，不過仍是搶鮮版 (Beta)。</td>
<td align="left">iOS 上沒有內建的繫結系統。 可透過使用第三方程式庫或撰寫額外的程式碼，以 <strong>Key-Value Observing (索引鍵值觀察)</strong> 為基礎進行建置，以執行資料繫結。 控制項使用委派/回呼方法來取得資料。</td>
<td align="left">UWP 平台可為您處理<strong>資料繫結</strong>。 您可以使用 <strong><a href="/windows/uwp/xaml-platform/x-bind-markup-extension">{x:Bind}</a></strong> 標記延伸來利用高效能系結或 <strong><a href="/windows/uwp/xaml-platform/binding-markup-extension">{binding}</a></strong> 來利用更多功能。 然後只需設定您的繫結，選擇平台要使用<strong>單向繫結</strong>在 UI 中顯示資料來源的值，或者使用<strong>雙向繫結</strong>一併觀察那些值並在值變更時更新您的 UI。<br/><br/><a href="/windows/uwp/data-binding/index">資料系結</a></td>
</tr>
<tr class="even">
<td align="left"><strong>使用者介面自動化。</strong> <br><br>以程式設計方式存取 UI 元素，讓輔助技術產品可以存取 app，以及讓自動化測試指令碼與您的 UI 互動。</td>
<td align="left"><strong>文字標籤</strong>、 <strong>contentDescription</strong> 和 <strong>提示</strong> 值有助於確保自動化可以找到 UI 元素。 Android Studio 可讓您使用 <strong>UI Automator</strong> 和 <strong>Espresso</strong> 測試架構撰寫 UI 測試。</td>
<td align="left"><strong>Automation instrument (自動化檢測)</strong> 可讓您撰寫自動化的 UI 測試指令碼，識別使用 <strong>accessibility (協助工具)</strong> 設定的元素或識別元素在 <strong>element hierarchy (元素階層)</strong> 中的位置。</td>
<td align="left">透過<strong><a href="/windows/desktop/WinAuto/uiauto-uiautomationoverview">使用者介面自動化</a></strong>，您能以程式設計方式直接存取 UWP 中的內建 UI 元素。<br/><strong><a href="/windows/uwp/accessibility/custom-automation-peers">自訂自動化對等</a></strong>可讓您為自己的自訂 UI 類別提供自動化支援。 Visual Studio 中的自動程式化 <strong><a href="/visualstudio/test/use-ui-automation-to-test-your-code?view=vs-2015">UI 測試專案</a></strong> 可讓您透過 UI 自動測試整個應用程式，或以隔離方式測試 UI。</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>變更控制項的外觀。</strong> <br><br>編輯大小、色彩與其他屬性。</td>
<td align="left">控制項包含可使用設計工具在 XML 標記中或以程式設計方式編輯的<strong>屬性 (property)</strong>。</td>
<td align="left">控制項包含可使用 Interface Builder (介面建立器) 中的 <strong>Attributes Inspector (屬性檢查程式)</strong> 或以程式設計方式編輯的<strong>屬性 (attribute)</strong>。</td>
<td align="left">您可以使用 Visual Studio 和 Blend for Visual Studio 以 XAML 標記或程式設計方式編輯控制項的<strong>屬性 (property)</strong>。<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">新增控制項和處理事件</a></td>
</tr>
<tr class="even">
<td align="left"><strong>可重複使用的視覺化樣式。</strong> <br><br>將視覺變更套用到多個控制項 (以可重複使用的格式)。</td>
<td align="left"><strong>XML</strong> 樣式 是套用到一或多個控制項的屬性 (property) 集。</td>
<td align="left">iOS 原生不支援可重複使用的視覺樣式，但是 UIAppearance 通訊協定可讓多個控制項共用通用屬性 (attribute)。</td>
<td align="left">您可以建立可重複使用的樣式，這些 <strong><a href="/uwp/api/Windows.UI.Xaml.Style">樣式</a></strong>可以套用至多個控制項，並儲存在 <strong><a href="/uwp/api/Windows.UI.Xaml.ResourceDictionary">ResourceDictionary</a></strong> 中方便重複使用。<br/><br/><a href="/previous-versions/windows/apps/hh465381(v=win.10)">快速入門：設定控制項的樣式</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>編輯控制項的視覺化結構。</strong> <br><br>自訂控制項的視覺化結構，而不只是修改屬性或屬性，例如，將核取方塊文字移到核取方塊下方。</td>
<td align="left">Android 中沒有編輯控制項之視覺結構的簡單方法。</td>
<td align="left">iOS 中沒有編輯控制項之視覺結構的簡單方法。</td>
<td align="left">若要自訂控制項的視覺化結構，您可以在 XAML 標記中複製和編輯其 <strong><a href="/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate">控制項範本</a></strong> 。<br/><br/><a href="/previous-versions/windows/apps/hh465374(v=win.10)">快速入門：控制項範本</a></td>
</tr>
<tr class="even">
<td align="left"><strong>內建觸控手勢。</strong> <br><br>透過處理高階抽象手勢事件 (例如在檢視和控制項中點選和點兩下)，可提供自訂的觸控支援。</td>
<td align="left"><strong>手勢偵測器</strong> 偵測一般觸控手勢，包括滾動、長按、點一下、點一下和 codingjar。</td>
<td align="left">UIKit 架構提供內建 <strong>gesture recognizer (手勢辨識器)</strong>，它會偵測觸控手勢，包括點選、捏合、移動瀏覽、撥動、旋轉和長按。</td>
<td align="left"><strong>UI 元素</strong> 可讓您處理 <strong>靜態手勢事件</strong> ，包括點一下、點一下、以滑鼠右鍵按一下和按住，以及 <strong>動作筆勢事件</strong> ，包括投影片、滑動、輪流、縮小和延展。 手勢事件是<strong>路由事件</strong>，而且可以由包含子 UIElement 的父物件處理。<br/><br/><a href="/windows/uwp/input-and-devices/touch-interactions">觸控互動</a><br/><br/><a href="/windows/uwp/design/layout/index">自訂使用者互動 - 手勢、操作及互動</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">瀏覽和 app 結構</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>佈局。</strong> <br><br>版面配置定義使用者介面的結構。</td>
<td align="left">版面配置是由 <strong>view group (檢視群組)</strong> (例如能以巢狀方式包含其他檢視群組或檢視的 <strong>LinearLayout</strong> 和 <strong>RelativeLayout</strong>) 組成。</td>
<td align="left">版面配置是由包含能以巢狀方式包含 <strong>UIView</strong> 的 <strong>UIViewController</strong> 組成。</td>
<td align="left">XAML，提供由 <strong>版面配置面板類別</strong> 組成的彈性版面配置系統，例如 <strong><a href="/uwp/api/windows.ui.xaml.controls.canvas">畫布</a></strong>、 <strong><a href="/uwp/api/windows.ui.xaml.controls.grid">方格</a></strong>、 <strong><a href="/uwp/api/windows.ui.xaml.controls.relativepanel">RelativePanel</a></strong> 和 <strong><a href="/uwp/api/windows.ui.xaml.controls.stackpanel">StackPanel</a></strong> ，用於靜態和回應式配置。 <strong><a href="/visualstudio/ide/reference/properties-window?view=vs-2015">屬性</a></strong>用來控制元素的大小和位置。<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">使用 XAML 定義版面配置</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>對等導覽。</strong> <br><br>向使用者顯示在階層同等重要的頁面之間瀏覽的方法。</td>
<td align="left"><strong>Tab (索引標籤)</strong>、<strong>swipe view (撥動檢視)</strong> 和 <strong>navigation drawer (瀏覽選單)</strong> 提供<strong>橫式瀏覽</strong>。</td>
<td align="left">索引標籤列<strong>控制器</strong>、<strong>分割視圖控制器</strong>和<strong>頁面流覽控制器</strong>允許在相等階層的視圖之間進行導覽。</td>
<td align="left">您可以使用 <strong><a href="/windows/uwp/controls-and-patterns/navigationview">NavigationView</a></strong>，在內容上方顯示連結/索引標籤的持續清單。 [ <strong><a href="/windows/uwp/controls-and-patterns/split-view">流覽] 窗格/[分割] 視圖</a></strong> 可讓您連同內容一起顯示連結清單。<br/><br/><a href="/windows/uwp/layout/navigation-basics">導覽</a><br/><br/><a href="/windows/uwp/layout/navigate-between-two-pages">在兩個頁面之間瀏覽</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>階層式導覽。</strong> <br><br>在階層的父項與子項頁面之間瀏覽。</td>
<td align="left"><strong>List (清單)</strong> 和 <strong>grid list (格線清單)</strong>、<strong>button (按鈕)</strong> 及其他控制項，搭配 <strong>intent (意圖)</strong> 使用時可提供 <strong>descendent navigation (下階瀏覽)</strong> 以載入其他 <strong>activity (活動)</strong>。</td>
<td align="left"><strong>流覽控制器</strong> 可讓使用者在階層的層級之間流覽。</td>
<td align="left"><strong><a href="/windows/uwp/controls-and-patterns/hub">中樞</a></strong>讓您為使用者顯示內容的預覽，使用者可以選取以瀏覽到子頁面。 <strong><a href="/windows/uwp/controls-and-patterns/master-details">主版/詳細</a></strong> 資料可讓使用者從 [對應的詳細資料] 區段旁邊顯示的專案摘要清單中挑選。<br/><br/><a href="/windows/uwp/layout/navigation-basics">導覽</a><br/><br/><a href="/windows/uwp/layout/navigate-between-two-pages">在兩個頁面之間瀏覽</a></td>
</tr>
<tr class="even">
<td align="left"><strong>[上一頁] 按鈕導覽。</strong> <br><br>在應用程式內往回瀏覽。</td>
<td align="left">動作列內的 [返回]<strong></strong> 和 [上一層]<strong></strong> 按鈕，可使用<strong>返回堆疊</strong>提供<strong>上階</strong>和<strong>暫時</strong>瀏覽。</td>
<td align="left"><strong>Navigation controller (瀏覽控制器)</strong> 可以新增返回按鈕。<br/></td>
<td align="left">您可以使用 [ <strong><a href="/uwp/api/windows.ui.xaml.controls.frame.backstack">back stack] 屬性</a></strong> （可讓您的使用者 <strong>流覽流覽歷程記錄</strong>），輕鬆地按下軟體或硬體的上一頁按鈕。<br/><br/><a href="/windows/uwp/layout/navigation-history-and-backwards-navigation">返回按鈕瀏覽</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>啟動顯示畫面。</strong> <br><br>在 app 啟動時顯示影像，主要用於顯示商標。</td>
<td align="left">預設不提供啟動顯示畫面，實作的方式是編輯第一個活動<strong>佈景主題背景</strong>。</td>
<td align="left">App 必須具有<strong>靜態啟動影像</strong>或 <strong>XIB/storyboard (腳本) 啟動檔案</strong>。</td>
<td align="left">您可以使用<strong>影像</strong>和彩色背景來建立啟動顯示畫面。 <a href="/windows/uwp/launch-resume/create-a-customized-splash-screen">啟動顯示畫面的時間可以延長</a>。<br/><br/><a href="/windows/uwp/launch-resume/add-a-splash-screen">新增啟動顯示畫面</a></td>
</tr>
</tbody>
</table>
<h2 id="custom-inputs">自訂輸入</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>聲音。</strong> <br><br>語音輸入的語音辨識及其他語音功能。</td>
<td align="left">實作 <strong>RecognizerIntent</strong> (例如 <strong>Google 語音搜尋</strong>) 的任何 app 都可提供語音輸入。 <strong>SpeechRecognizer</strong> 類別可以讓 app 使用 Google 的語音辨識 API。</td>
<td align="left">應用程式可以使用 <strong>SFSpeechRecognizer</strong> 類別來實作語音輸入以及語音辨識。</td>
<td align="left">您可以使用<strong><a href="/windows/uwp/input-and-devices/speech-recognition">語音辨識</a></strong> API，與 App 在前景進行互動。 您可以使用語音型 <strong><a href="/windows/uwp/input-and-devices/cortana-interactions">Cortana 互動</a></strong>在前景或背景啟動應用程式，以及與背景應用程式互動。<br/><br/><a href="/windows/uwp/input-and-devices/speech-interactions">語音互動</a></td>
</tr>
<tr class="even">
<td align="left"><strong>自訂使用者輸入。</strong> <br><br>處理鍵盤、滑鼠、手寫筆及其他輸入方式。</td>
<td align="left">對互動的支援包括<strong>觸控</strong>、<strong>觸控板</strong>、<strong>手寫筆</strong>、<strong>滑鼠</strong>和<strong>鍵盤</strong>。 移動和輸入的報告方式與觸控相同，但是可能會偵測到有關<strong>輸入裝置</strong>的更多資訊。</td>
<td align="left">提供對<strong>觸控</strong>、<strong>Apple Pencil</strong> 和硬體<strong>鍵盤</strong>的支援。</td>
<td align="left">您將可以找到各種互動的支援，包括<strong><a href="/windows/uwp/input-and-devices/touch-interactions">觸控</a></strong>、觸控<strong><a href="/windows/uwp/input-and-devices/touchpad-interactions">板</a></strong>、使用數位筆跡、<strong><a href="/windows/uwp/input-and-devices/mouse-interactions">滑鼠</a></strong>和<strong><a href="/windows/uwp/input-and-devices/keyboard-interactions">鍵盤</a></strong>的<strong><a href="/windows/uwp/input-and-devices/pen-and-stylus-interactions">手寫筆/手寫筆</a></strong>。 您的 app 可以處理資料，不需要知道使用了哪種輸入裝置，如有需要，也可以存取原始輸入裝置資料。<br/><br/><a href="/windows/uwp/input-and-devices/handle-pointer-input">處理指標輸入</a><br/><br/><a href="/windows/uwp/design/layout/index">自訂使用者互動</a></td>
</tr>
</tbody>
</table>
<h2 id="data">資料</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>本機應用程式資料。</strong> <br><br>將與 app 相關的設定和檔案儲存在本機。</td>
<td align="left">使用 <strong>openFileOutput</strong> 和 <strong>openFileInput</strong> 可以儲存本機檔案。 使用 <strong>getSharedPreferences</strong> 可存取<strong>共用喜好設定檔案</strong>中的設定。</td>
<td align="left">本機檔案可以儲存在 <strong>application support (應用程式支援)</strong> 目錄中，並透過 <strong>NSFileManager</strong> 類別存取。 <strong>Preferences (喜好設定)</strong> 檔案中的設定可以透過 <strong>NSUserDefaults</strong> 類別存取。</td>
<td align="left"><strong><a href="/uwp/api/Windows.Storage">Windows.Storage</a></strong> 類別可利用統一的方式為您處理本機資料儲存區。 您可以將設定儲存為 <strong><a href="/uwp/api/Windows.Storage.ApplicationDataContainer">windows.storage.applicationdatacontainer</a></strong> 物件，並透過 <strong><a href="/uwp/api/windows.storage.applicationdata.localsettings">ApplicationData LocalSettings</a></strong> 屬性來存取。 您可將檔案儲存在 <strong><a href="/uwp/api/windows.storage.storagefolder">StorageFolder</a></strong> 物件中，並透過 <strong><a href="/uwp/api/windows.storage.applicationdata.localfolder">ApplicationData.LocalFolder</a></strong> 屬性存取。<br/><br/><a href="/windows/uwp/app-settings/store-and-retrieve-app-data">儲存及擷取設定和其他應用程式資料</a></td>
</tr>
<tr class="even">
<td align="left"><strong>本機資料庫儲存體。</strong> <br><br>將 app 資料儲存在關聯式資料庫，如果適用，請使用具有物件關聯式對應程式 (ORM) 功能的關聯式資料庫。</td>
<td align="left">提供 <strong>SQLite</strong> 資料庫。 沒有內建 ORM。 使用 <strong>SQLiteDatabase</strong> 類別執行 SQL 查詢。</td>
<td align="left">提供 <strong>SQLite</strong> 資料庫。 <strong>CoreData</strong> 是可與 SQLite 搭配使用的內建物件圖形架構，它提供相當於 ORM 的功能。</td>
<td align="left">您可以使用 <strong>SQLite</strong> 存放資料。 <strong><a href="/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps">Entity Framework</a></strong> 是內建的 ORM，可免除寫入大量資料存取程式碼的需求，並可讓您輕鬆地查詢資料庫，而不需要撰寫 SQL。 您可以使用 <a href="/windows/uwp/data-access/sqlite-databases">SQLite 程式庫</a>直接執行 SQL 查詢。<br/><br/><a href="/windows/uwp/data-access/index">資料存取</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>REST 存取的 HTTP 程式庫。</strong> <br><br>讓您能夠使用 HTTP(S) 與 Web 服務及網頁伺服器通訊的內建程式庫。<br/></td>
<td align="left">HTTP 程式庫 <strong>HttpURLConnection</strong> 和<strong>Volley</strong>。</td>
<td align="left"><strong>NSURLSession</strong>、<strong>NSURLConnection</strong> 和 <strong>NSURLDownload</strong>。</td>
<td align="left">您可以使用內建的 <strong><a href="/uwp/api/Windows.Web.Http.HttpClient">HttpClient</a></strong> API 來存取常見的 HTTP 功能，包括 GET、DELETE、PUT、POST、一般驗證模式、SSL、cookie 和進度資訊。</td>
</tr>
<tr class="even">
<td align="left"><strong>雲端備份服務。</strong> <br><br>平台為 app 資料提供的備份服務。</td>
<td align="left">Android 的 <strong>backup manager (備份管理員)</strong> 可以處理 Google 的 <strong>Android Backup Service (Android 備份服務)</strong> 中應用程式資料的備份。</td>
<td align="left"><strong>ICloud 備份</strong> 可以由使用者設定來處理其備份，包括應用程式資料。 使用與 iCloud 相容的 <strong>Core Data (核心資料)</strong>、<strong>iCloud key-value store (iCloud 索引鍵值存放區)</strong> 和 <strong>iCloud document storage (iCloud 文件儲存區)</strong> 的 App。</td>
<td align="left">使用漫遊 <strong><a href="/uwp/api/windows.storage.applicationdata">ApplicationData API</a></strong> (包括 <strong><a href="/uwp/api/windows.storage.applicationdata.roamingfolder">RoamingFolder</a></strong> 和 <a href="/uwp/api/windows.storage.applicationdata.roamingsettings"><strong>RoamingSettings</strong></a>) 儲存區的任何 app 資料，都會自動同步到雲端，也會自動同步到使用者的其他裝置。 同步是利用使用者的 Microsoft 帳戶來完成。<br/><br/><a href="/windows/uwp/design/app-settings/store-and-retrieve-app-data">app 資料漫遊的指導方針</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>HTTP 檔案下載。</strong> <br><br>透過 HTTP 下載大型和小型檔案。</td>
<td align="left"><strong>URLConnection</strong> 和 <strong>HTTPURLConnection</strong> 是用來透過 HTTP 和 FTP 進行下載，也可以使用系統 <strong>下載管理員</strong> 在背景中下載。</td>
<td align="left"><strong>NSURLSession</strong> 和 <strong>NSURLConnection</strong> 可以用來透過 HTTP 和 FTP 下載檔案。</td>
<td align="left"><strong><a href="/uwp/api/windows.networking.backgroundtransfer">背景傳輸 API</a></strong> 可讓您透過 HTTP(S) 和 FTP 可靠地傳輸檔案，顧及 app 暫停、連線中斷，並根據連線能力和電池使用時間進行調整。 您也可以使用適用于較小檔案的 <strong><a href="/uwp/api/windows.web.http.httpclient">HttpClient</a></strong> 。<br/><br/><a href="/windows/uwp/networking/which-networking-technology">哪一種網路功能技術？</a><br/><br/><a href="/windows/uwp/networking/background-transfers">背景傳輸</a></td>
</tr>
<tr class="even">
<td align="left"><strong>插座。</strong> <br><br>建立低層級的 UDP 資料包和 TCP 通訊端，使用您自己的通訊協定與其他裝置通訊。</td>
<td align="left"><strong>Socket</strong> 類別提供 TCP 通訊端， <strong>DATAGRAMSOCKET</strong> 類別提供 UDP 通訊端。</td>
<td align="left"><strong>NSStream</strong> 和 <strong>CFStream</strong> 提供 TCP 通訊端，而 <strong>CFSocket</strong> 提供 UDP 通訊端。</td>
<td align="left">您可以使用 <strong><a href="/uwp/api/Windows.Networking.Sockets.DatagramSocket">DatagramSocket</a></strong> 類別透過 UDP 資料包通訊端進行通訊，以及使用 <strong><a href="/uwp/api/Windows.Networking.Sockets.StreamSocket">StreamSocket</a></strong> 類別透過 TCP 或藍牙 RFCOMM 進行通訊。<br/><br/><a href="/windows/uwp/networking/networking-basics">網路功能基本知識</a><br/><br/><a href="/windows/uwp/networking/which-networking-technology">哪一種網路功能技術？</a><br/><br/><a href="/windows/uwp/networking/sockets">通訊端概觀</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Websocket.</strong> <br><br>在用戶端與伺服器之間提供雙向通訊，實現即時資料傳輸。</td>
<td align="left">Android 上沒有內建 WebSockets 程式庫。</td>
<td align="left">iOS 上沒有內建 WebSockets 程式庫。</td>
<td align="left">您可以透過下面的類別建立與支援 WebSocket 之伺服器間的安全連線<strong><a href="/uwp/api/windows.networking.sockets.messagewebsocket">MessageWebSocket</a></strong> 類別 (適用具有接收通知的小型訊息)，以及 <strong><a href="/uwp/api/windows.networking.sockets.streamwebsocket">StreamWebSocket</a></strong> (適用於可分段讀取的較大二進位檔案傳輸)。<br/><br/><a href="/windows/uwp/networking/networking-basics">網路功能基本知識</a><br/><br/><a href="/windows/uwp/networking/which-networking-technology">哪一種網路功能技術？</a><br/><br/><a href="/windows/uwp/networking/websockets">Websocket 概觀</a></td>
</tr>
<tr class="even">
<td align="left"><strong>OAuth 程式庫。</strong> <br><br>OAuth 程式庫允許存取第三方 OAuth 提供者，以及平台中內建的任何帳戶管理。</td>
<td align="left">不提供一般的 OAuth 程式庫。 針對 Google Play 服務的 OAuth 驗證提供 <strong>GoogleAuthUtil</strong> 類別。<br/></td>
<td align="left">不提供一般的 OAuth 程式庫。 <strong>帳戶架構</strong> 提供對裝置上已經儲存之使用者帳戶 (例如 Facebook 和 Twitter) 的存取權。</td>
<td align="left">一般 OAuth 程式庫 <strong><a href="/windows/uwp/security/web-authentication-broker">Web 驗證訊息代理</a></strong> 程式可讓您連線至協力廠商識別提供者服務。 <strong><a href="/windows/uwp/security/credential-locker">認證保險箱</a></strong>可讓使用者儲存他們的登入並在多個裝置上使用。 <strong><a href="/previous-versions/office/developer/onedrive-live-sdk-reference/dn896755(v=office.15)">Microsoft.Live</a></strong> 命名空間可以讓您輕鬆地存取 Live SDK OAuth 以存取 Microsoft 服務。<br/><br/><a href="/windows/uwp/security/authentication-and-user-identity">驗證和使用者識別</a><br/><br/><a href="/uwp/api/windows.security.authentication.web">Windows.Security.Authentication.Web API 文件</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">WebAuthenticationBroker 程式碼範例</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">Tooling</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Ide。</strong> <br><br>用來建立 app 的工具組。</td>
<td align="left"><strong>Android studio</strong> 和 <strong>Eclipse</strong>，讓 Google 將開發人員導向使用 Android studio。</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://visualstudio.microsoft.com/features/universal-windows-platform-vs">Visual studio</a></strong> 和 <strong><a href="/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend for visual studio</a></strong> 擁有您撰寫程式碼、設計、連接、偵測、分析、優化及測試 UWP 應用程式所需的所有工具。 Visual Studio 也為您提供適用於 Windows 10 裝置的<strong><a href="/windows/uwp/debug-test-perf/test-with-the-emulator">模擬器</a></strong>，以便您在各種模擬的裝置上測試 app。<br/><br/><a href="https://developer.microsoft.com/windows/downloads">適用於 UWP 的下載項目與工具</a></td>
</tr>
<tr class="even">
<td align="left"><strong>程式碼組織。</strong> <br><br>App 的基本資料夾結構 (通常從初始範本建立)。</td>
<td align="left"><strong>AndroidManifest</strong> 檔案、包含來源檔案的 <strong>java</strong> 資料夾、包含資源 (含版面配置和值) 的 <strong>res</strong> 資料夾、Android Studio 中的 <strong>Gradle</strong> 建置指令碼，以及 Eclipse 中的 <strong>Ant</strong> 建置指令碼。</td>
<td align="left">來源檔案和 <strong>Supporting Files (支援檔案)</strong>、<strong>Info.plist</strong> 檔案、<strong>Main.storyboard</strong> 以及 <strong>LaunchScreen.storyboard</strong>。 影像會儲存在 <strong>Asset libraries (資產庫)</strong> 中。</td>
<td align="left">您的 UWP app 包含 XAML 及名為 Example.xaml 和 Example.xaml.cs 的程式碼檔案、<strong>Assets 資料夾</strong>中的各種影像、開始頁面 (例如 <strong>MainPage.xaml</strong> 和 <strong>MainPage.xaml.cs</strong>) 以及資訊清單。<br/><br/><a href="/windows/uwp/get-started/create-a-hello-world-app-xaml-universal">建立 hello world 應用程式</a></td>
</tr>
</tbody>
</table>
<h2 id="app-lifecycle">應用程式週期</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>應用程式生命週期。</strong> <br><br>處理 app 啟動、暫停、繼續和關閉的事件，讓您可以儲存/還原應用程式狀態並執行其他工作。</td>
<td align="left">每個活動都有自己的<strong>活動週期</strong>，且具有<strong>已繼續</strong>之類的狀態。 <strong>生命週期回呼</strong> （例如 <strong>onResume</strong> ）會在您的 <strong>活動類別</strong>中執行。</td>
<td align="left"><strong>應用程式週期</strong>具有<strong>已暫停</strong>之類的狀態。 <strong>applicationDidEnterBackground:</strong> 之類的方法是在<strong>應用程式委派物件</strong>中實作，可在狀態變更時執行程式碼。</td>
<td align="left">您的 app 具有 NotRunning、Activated、Running、Suspending、Suspended 及 Resuming 的 <strong>app 執行狀態</strong>。<br/><br/>您可以在應用程式中執行 >onlaunched、OnActivated、暫停或繼續的 <strong><a href="/uwp/api/windows.ui.xaml.application">應用程式類別</a></strong> 方法，以在狀態變更時執行程式碼。<br/><br/><a href="/windows/uwp/launch-resume/app-lifecycle">應用程式生命週期</a></td>
</tr>
<tr class="even">
<td align="left"><strong>背景工作。</strong> <br><br>執行背景作業的工作以及當 app 不再在前景時繼續執行的工作。</td>
<td align="left">當 app 不再在前景時，app 可以啟動執行背景作業的<strong>服務</strong>。 服務有自己<strong>週期</strong>，而且已登錄到資訊清單中。</td>
<td align="left">只有特定工作類型才允許<strong>背景執行</strong>。<br/><br/>App 使用 <strong>UIBackgroundModes</strong> 在 Info.plist 檔案中宣告<strong>支援的背景工作</strong>。<br/><br/>系統會控制背景工作的執行時間和持續時間。</td>
<td align="left">您可以藉由執行 <strong><a href="/uwp/api/windows.applicationmodel.background.ibackgroundtask">IBackgroundTask</a></strong> 介面並在應用程式資訊清單中註冊工作，來建立背景工作。 您可以設定要使用 <a href="/windows/uwp/launch-resume/run-a-background-task-on-a-timer-"><strong>計時器</strong></a>、 <a href="/uwp/api/windows.applicationmodel.background.systemtriggertype"><strong>系統觸發</strong></a>程式和 <a href="/windows/uwp/launch-resume/use-a-maintenance-trigger"><strong>維護觸發</strong></a>程式觸發的工作。<br/><br/><a href="/windows/uwp/launch-resume/support-your-app-with-background-tasks">使用背景工作支援應用程式</a><br/><br/><a href="/windows/uwp/launch-resume/create-and-register-a-background-task">建立並註冊背景工作</a><br/><br/><a href="/windows/uwp/launch-resume/guidelines-for-background-tasks">背景工作的指導方針</a></td>
</tr>
</tbody>
</table>
<h2 id="performance">效能</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>效能最佳做法。</strong> <br><br>建置執行速度快、回應靈敏、啟動時間短且可延長電池使用時間之 app 的指導方針。</td>
<td align="left">Android 提供 <strong>Best Practices for Performance (效能的最佳做法)</strong> 訓練指南。</td>
<td align="left">iOS 提供 <strong>Performance Overview (效能概觀)</strong> 文件。</td>
<td align="left">您可以閱讀詳細的<strong><a href="/windows/uwp/debug-test-perf/performance-and-xaml-ui">效能指南</a></strong>中涵蓋下列主題的章節：設定效能目標、測量效能、記憶體管理、流暢的動畫、高效率的檔案系統存取，以及用於分析和效能的工具。</td>
</tr>
<tr class="even">
<td align="left"><strong>查看回應式 UI 的優化。</strong> <br><br>透過將檢視最佳化以改善效能。</td>
<td align="left">使用 Hierarchy Viewer (階層檢視器) 工具最佳化<strong>版面配置階層</strong>、<strong>重複使用版面配置</strong>和<strong>隨需載入檢視</strong>，都是可以協助保持 UI 執行緒回應性和避免 [應用程式未回應]&quot;&quot; 對話方塊 (<strong>ANR</strong>) 的技術。<br/></td>
<td align="left">修正<strong>螢幕範圍以外的轉譯</strong>、<strong>混合層</strong>、<strong>點陣化</strong>的 UI 問題，使用 <strong>Core Animation (核心動畫)</strong> 工具協助讓 UI 執行緒保持回應。</td>
<td align="left">依照幾個簡單步驟操作，即可輕鬆地<strong>最佳化</strong> XAML <strong>標記</strong>和<strong>版面配置</strong>。 這些技術包括精簡版面配置結構、盡量減少元素數目及過度繪製。 <br/><br/><a href="/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive">讓 UI 執行緒保持回應</a><br/><br/><a href="/windows/uwp/debug-test-perf/optimize-xaml-loading">最佳化您的 XAML 標記</a><br/><br/><a href="/windows/uwp/debug-test-perf/optimize-your-xaml-layout">最佳化您的 XAML 配置</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>執行緒。</strong> <br><br>使用執行緒處理來維護<strong>回應式 UI</strong> 並執行多個<strong>平行工作</strong>。</td>
<td align="left">實現執行緒處理的方法是使用 <strong>Runnable</strong>、<strong>Handler</strong>、<strong>ThreadPoolExecutor</strong> 及更高階的 <strong>AsyncTask</strong> 類別。</td>
<td align="left">實現執行緒處理的方法是使用 <strong>NSThread</strong>、<strong>Grand Central Dispatch</strong> 及更高階的 <strong>NSOperation</strong>。</td>
<td align="left">您可以使用<strong><a href="/uwp/api/windows.system.threading.threadpool.runasync">RunAsync</a></strong>將<strong>工作專案</strong>提交給<strong>threadpool</strong> ，以使用執行緒。 您可以使用計時器來提交包含 <strong><a href="/uwp/api/windows.system.threading.threadpooltimer.createtimer">>createtimer</a></strong> 的工作專案，並使用 <strong><a href="/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer">CreatePeriodicTimer</a></strong>建立重複的工作專案。<br/><br/><a href="/windows/uwp/threading-async/submit-a-work-item-to-the-thread-pool">將工作項目提交至執行緒集區</a><br/><br/><a href="/windows/uwp/threading-async/use-a-timer-to-submit-a-work-item">使用計時器提交工作項目</a><br/><br/><a href="/windows/uwp/threading-async/create-a-periodic-work-item">建立定期工作項目</a><br/><br/><a href="/windows/uwp/threading-async/best-practices-for-using-the-thread-pool">使用執行緒集區的最佳做法</a></td>
</tr>
<tr class="even">
<td align="left"><strong>非同步程式設計。</strong> <br><br>利用非同步程式設計模式避免執行緒處理的複雜度，以便讓 UI 執行緒保持回應。</td>
<td align="left">建立您自己的非同步類別時，<strong>必須使用執行緒處理</strong>。 某些內建類別是非同步的。</td>
<td align="left">建立您自己的非同步類別時，<strong>必須使用執行緒處理</strong>。 某些內建類別是非同步的。</td>
<td align="left">當您建立自己的 Api （例如，在 c # 和 Visual Basic 中使用 <strong>async</strong> 和 <strong>await</strong> ）時，您可以使用非同步模式來避免封鎖主執行緒。 您可以使用結尾為文字 <strong>Async</strong> 的非同步內建 API。<br/><br/><a href="/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">非同步程式設計</a><br/><br/><a href="/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic">在 C# 或 Visual Basic 中呼叫非同步 API</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>清單視圖優化。</strong> <br><br>用來協助將資料清單最佳化的內建模式，這種清單需要顯示大量資料時，通常會發生效能不佳的情況</td>
<td align="left"><strong>ViewHolder</strong> 設計模式是用來避免多個檢視查閱，可讓您使用重複使用的 UI 元素。</td>
<td align="left">可進行一些最佳化以改善 <strong>UITableView</strong> 的效能，沒有任何內建項目。</td>
<td align="left">您可以使用直接提供 <strong>UI 虛擬化</strong>的 <a href="/uwp/api/windows.ui.xaml.controls.listview">ListView</a> 和 <a href="/uwp/api/windows.ui.xaml.controls.gridview">GridView</a> 控制項，它們提供順暢的移動瀏覽和捲動體驗及較快的啟動時間。 您也可以在資料來源中實作 <a href="/dotnet/api/system.collections.ilist">IList</a> 和 <a href="/dotnet/api/system.collections.specialized.inotifycollectionchanged">INotifyCollectionChanged</a>，它們提供<strong>資料虛擬化</strong>並進一步提升效能。<br/><br/><a href="/windows/uwp/debug-test-perf/optimize-gridview-and-listview">ListView 與 GridView UI 最佳化</a><br/><br/><a href="/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization">ListView 與 GridView 資料虛擬化</a></td>
</tr>
</tbody>
</table>
<h2 id="monetization">創造營收</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>應用程式內購買。</strong> <br><br>允許使用者在您的 app 中進行購買的平台功能。</td>
<td align="left"><strong>應用程式內帳單</strong> 是由 Google 服務所提供。 產品會新增至 <strong>Google Play 開發人員主控台</strong>。 App 內購買使用 <strong>Google Play Billing Library (Google Play 帳單功能程式庫)</strong> 實作。</td>
<td align="left">產品已新增至 <strong>iTunes Connect</strong>。 App 內購買使用 <strong>StoreKit</strong> 架構實作。<br/><br/>產品的購買使用 <strong>SKMutablePayment</strong> 和 <strong>SKPaymentQueue</strong>。</td>
<td align="left">您<a href="/windows/uwp/publish/iap-submissions">將應用程式內產品購買新增到 app 並提交到Microsoft Store</a>，即可為 app 建立它們。 <br/><br/>您使用 <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp">CurrentApp 類別</a></strong>來定義 App 內購買。 <br/><br/>您可以使用 <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync">CurrentApp RequestProductPurchaseAsync</a></strong> 來顯示允許客戶購買產品的 UI。<br/><br/><a href="/windows/uwp/monetize/enable-in-app-product-purchases">啟用應用程式內產品購買</a></td>
</tr>
<tr class="even">
<td align="left"><strong>取用應用程式內購買。</strong> <br><br>可以購買、使用並再次購買的應用程式內產品。</td>
<td align="left">消耗性物品購買是使用 <strong>consumePurchase</strong> 定期購買然後消耗，讓使用者能夠購買、使用並再次購買。</td>
<td align="left">消耗性物品在 iTunes Connect 中是<strong>定義為消耗性物品</strong>。</td>
<td align="left"><a href="/windows/uwp/publish/enter-iap-properties">當您將產品類型定義為「消耗性物品」並提交到市集</a>時，即可支援消耗性物品。 接著，您可以在有人購買消耗性物品之後呼叫 <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync">CurrentApp.ReportConsumableFulfillmentAsync</a></strong>，以允許客戶存取它。<br/><br/><a href="/windows/uwp/monetize/enable-consumable-in-app-product-purchases">啟用在應用程式內購買消耗性物品</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>測試應用程式內購買。</strong> <br><br>讓您無需將 app 放置在Microsoft Store，即可測試在應用程式內購買的程式碼。</td>
<td align="left"><strong>App 內帳單功能沙箱</strong>用於測試。</td>
<td align="left"><strong>沙箱測試人員帳戶</strong>用於測試。</td>
<td align="left">只要使用 <strong><a href="/uwp/api/windows.applicationmodel.store.currentappsimulator">CurrentAppSimulator</a></strong> 類別取代 CurrentApp，即可測試 App 內購買。<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>試驗。</strong> <br><br>讓您在 app 試用版上輕鬆地限制內容或移除廣告。</td>
<td align="left">Google Play <strong>沒有正式支援 app 試用版</strong>。 試用版或移除廣告是透過建立在應用程式內購買和在確認購買成功時採取適當的程式碼路徑來達成的。</td>
<td align="left">App Store <strong>沒有正式支援 app 試用版</strong>。 試用版或移除廣告是透過建立在應用程式內購買和在確認購買成功時採取適當的程式碼路徑來達成的。</td>
<td align="left">您可以在將應用程式提交至商店時，使用 [ <strong><a href="/windows/uwp/publish/set-app-pricing-and-availability">免費試用] 選項</a></strong> 來提供應用程式的免費試用版。 然後，您可以使用 <strong><a href="/uwp/api/windows.applicationmodel.store.licenseinformation.istrial">LicenseInformation</a></strong> 來檢查應用程式的試用狀態，並據以呈現不同的程式碼路徑。 您也可以登錄 <a href="/uwp/api/windows.applicationmodel.store.licenseinformation.licensechanged">LicenseChanged 事件</a>，在使用者於 app 正在執行時變更試用狀態時接收通知。<br/><br/><a href="/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app">在試用版本中排除或限制某些功能</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">適應多個平台</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>自動調整 UI：彈性的版面配置。</strong> <br><br>使用彈性的高度和寬度支援不同的螢幕大小。</td>
<td align="left">使用 LinearLayout 物件中的 <strong>wrap_content</strong> 和 <strong>match_parent</strong> 值，或是使用用於對齊的 RelativeLayout 物件，可建立彈性版面配置。</td>
<td align="left">使用 <strong>adaptive model (調適型模型)</strong> 搭配通用 Storyboard (腳本)、使用 <strong>Auto Layout (自動版面配置)</strong> 搭配 <strong>constraints (限制式)</strong> 和 <strong>traits (特性)</strong> (例如 horizontalSizeClass 和 displayScale，它們可套用到檢視控制器)，可建立彈性版面配置。</td>
<td align="left">使用<strong>版面配置屬性</strong>和<strong>面板</strong>搭配固定和動態調整大小的組合，可建立流暢版面配置。<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">使用 XAML 定義版面配置 - 版面配置屬性與面板</a><br/><br/><a href="/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">回應式設計 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>適應性 UI：量身打造的版面配置。</strong> <br><br>使用適用目標不同的個別版面配置來支援不同的螢幕大小。</td>
<td align="left">使用<strong>設定限定詞</strong>，例如 <strong>small</strong>、<strong>large</strong>、<strong>ldpi</strong> 和 <strong>hdpi</strong>，讓您針對各種大小和密度的螢幕自訂版面配置，在資源目錄中為不同的螢幕設定提供替代的版面配置檔案。</td>
<td align="left">定義<strong>個別的 iPhone 和 iPad Storyboard (腳本)</strong>，在通用 app 中針對不同裝置系列量身打造版面配置。</td>
<td align="left">依每個裝置系列定義<strong>不同的 XAML 標記檔案</strong>，可建置量身訂做的版面配置。<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">使用 XAML 定義版面配置 - 量身訂做的版面配置</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>自動調整 UI：回應式版面配置。</strong> <br><br>回應螢幕大小的變更 (例如旋轉螢幕) 或視窗大小的變更。</td>
<td align="left">使用彈性版面配置搭配 <strong>LinearLayout</strong> 和 <strong>RelativeLayout</strong>，或針對不同的方向提供替代版面配置檔案以啟用回應式版面配置。</td>
<td align="left">當檢視的 <strong>size (大小)</strong> 或 <strong>traits (特性)</strong> 變更時，會套用 storyboard (腳本) 中指定的 <strong>constraint (限制式)</strong>。</td>
<td align="left">使用 <strong><a href="/uwp/api/windows.ui.xaml.visualstate">VisualState</a></strong>、<strong><a href="/uwp/api/windows.ui.xaml.visualstatemanager">VisualStateManager</a></strong> 和 <strong><a href="/uwp/api/windows.ui.xaml.adaptivetrigger">AdaptiveTrigger</a></strong>，可輕鬆在執行階段自動重排、重新定位、調整大小、顯示或取代 UI 區段以回應視窗大小變更。<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">使用 XAML 定義版面配置 - 視覺狀態和狀態觸發程序</a><br/><br/><a href="/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">回應式設計 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>支援不同的裝置功能。</strong> <br><br>利用進階的硬體功能，同時也支援沒有這些功能的裝置。</td>
<td align="left">使用 <strong>PackageManager.hasSystemFeature</strong> 在執行階段測試裝置功能，可讓您決定硬體特定程式碼是否可以執行。</td>
<td align="left">沒有可在執行階段測試裝置功能的<strong>單一檢查</strong>，您必須以特定方式測試每個功能，以決定硬體特定程式碼是否可以執行。</td>
<td align="left">您可以新增<strong>平台擴充功能 SDK</strong> 到您的套件，以針對不同目標裝置系列 (例如手機、電腦及 IoT) 中的額外功能提供支援。 您可以使用 <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation">APIINFORMATION API</a></strong> 來測試回合時間的型別和成員是否存在，而且只有在存在這些型別和成員時，才可以呼叫這些類型和成員。</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>支援不同的裝置功能。</strong> <br><br>利用進階的硬體功能，同時也支援沒有這些功能的裝置。</td>
<td align="left"><strong>Android Support Library (Android 支援程式庫)</strong> 可以封裝到您的 app 中，讓舊版的 Android 也可以使用一些較新的 API。 使用 <strong>Build.Version.SDK_INT</strong> 可在執行階段測試 API 層級。</td>
<td align="left">標準的執行階段檢查是用來了解 API 是否可用，例如，<strong>class</strong> 方法是用來檢查類別是否存在，而 <strong>respondsToSelector:</strong> 是用來檢查類別上的方法。</td>
<td align="left">您可以使用 <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent">ApiInformation.IsApiContractPresent</a></strong>，識別是否存在指定的主版本和副版本數字的 API 協定。 您也可以使用 <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation">APIINFORMATION API</a></strong> ，在執行時間測試類型和成員是否存在，而且只有在存在這些類型和成員時，才能呼叫這些類型和成員。</td>
</tr>
</tbody>
</table>
<h2 id="notifications">通知</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>磚與徽章。</strong> <br><br>在主畫面上對使用者顯示更新。</td>
<td align="left"><strong>應用程式 widget 是您</strong> 應用程式上可內嵌至主畫面並可接收定期更新的視圖。 Android 上<strong>沒有徽章系統</strong>存在。 此外，也沒有與磚完全一樣的系統。</td>
  <td align="left">iOS 上的 <strong>Widget</strong>出現在通知中心且實作為<strong>應用程式擴充功能</strong>。 您也可以新增包含數字的<strong>徽章</strong>到圖示，這個數字會隨著本機或遠端通知的數目而變更。 沒有磚系統。</td>
<td align="left">您的 app 有可以釘選到開始畫面的<strong>磚</strong>，您用它來顯示您所選的文字、影像，以及包含字符和數字的<strong>徽章</strong>。 您可以從 app 更新磚的內容 (透過推播通知或根據預先定義的排程)。 磚可以具有彈性，根據顯示的位置來變更。<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">建立磚</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-create-adaptive-tiles">建立彈性磚</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">選擇通知傳遞方法</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">磚與徽章的指導方針</a></td>
</tr>
<tr class="even">
<td align="left"><strong>顯示通知。</strong> <br><br>可以顯示的通知類型。</td>
<td align="left">通知可以顯示在<strong>通知區域</strong>和<strong>通知選單</strong>，<strong>預告通知</strong>會在小型的浮動視窗中顯示通知。 定義 <strong>PendingIntent</strong> 可在通知中新增動作。</td>
<td align="left">快顯通知會顯示為<strong>橫幅</strong>或<strong>警示</strong>。 您可以新增自訂動作按鈕到<strong>可執行動作的通知</strong>，該通知是使用 <strong>UIMutableUserNotificationAction</strong> 來定義的。</td>
<td align="left">您可以建立稱為<strong>快顯通知</strong>的調適型快顯通知。 您可以在 XML 中定義包含視覺化內容、<strong>動作</strong> (可以是按鈕) 或輸入和音訊的快顯通知。<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">調適型和互動式快顯通知</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">選擇通知傳遞方法</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-badges-notifications">快顯通知的指導方針</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>排程本機通知。</strong> <br><br>由您的 app 在排定的時間傳送的本機通知。</td>
<td align="left">通知與動作是使用 <strong>NotificationCompat.Buildr</strong> 來定義的，而且可以在 app 中使用 <strong>AlarmManager</strong> 和 <strong>BroadcastReceiver</strong> 來排程並處理。</td>
<td align="left">本機通知是使用 <strong>UILocalNotification</strong> 所建立，可以透過 <b>UILocalNotification.scheduleLocalNotification: <strong>進行排程。| 您可以使用 </strong><a href="/uwp/api/Windows.UI.Notifications.ScheduledToastNotification">ScheduledToastNotification</a><strong> 排程快顯通知。您可以使用 </strong><a href="/uwp/api/Windows.UI.Notifications.TileNotification">TileNotification 類別</a><strong>從 App 傳送，或是透過 <a href="/uwp/api/Windows.UI.Notifications.ScheduledTileNotification">ScheduledTileNotification</a> 排程磚通知。<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">調適型和互動式快顯通知</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-sending-a-local-tile-notification">傳送本機磚通知</a> | |正在傳送 </strong> 推播通知。</b> 從推播通知伺服器傳送，而且可選擇在 App 內處理的通知。</td>
<td align="left"><strong>Google 雲端通訊</strong> 提供適用于 Android 的推播通知支援。</td>
</tr>
</tbody>
</table>
<h2 id="media-capture-and-rendering">媒體擷取與轉譯</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>正在捕獲媒體。</strong> <br><br>錄製音訊和視覺化內容。</td>
<td align="left">使用 <strong>intent</strong> (例如 MediaStore.ACTION_VIDEO_CAPTURE) 可利用現有的相機 app 擷取媒體。 使用 <strong>android.hardware.camera2</strong> 或 <strong>camera</strong> 程式庫可實作自訂相機介面。 <strong>MediaRecorder</strong> Api 可用於捕獲音訊。</td>
<td align="left"><strong>UIImagePickerController</strong> 允許使用系統 UI 擷取視訊和相片。 <strong>AVFoundation</strong> 類別 (例如 <strong>AVCaptureSession</strong>) 可啟用對相機的直接存取。 <br/><strong>AVAudioRecorder</strong> 類別可啟用音訊錄製。</td>
<td align="left">您可以使用內建的攝影機 UI 搭配 <strong><a href="/uwp/api/Windows.Media.Capture.CameraCaptureUI">CameraCaptureUI 類別</a></strong>來捕捉相片和影片。 您可以與相機進行低階互動，並使用 <strong><a href="/uwp/api/Windows.Media.Capture">Windows.Media.Capture</a></strong> 中的類別 (例如 <strong><a href="/uwp/api/Windows.Media.Capture.MediaCapture">MediaCapture API</a></strong>) 來擷取音訊。 <br/><br/><a href="/windows/uwp/audio-video-camera/capture-photos-and-video-with-cameracaptureui">使用 CameraCaptureUI 擷取相片和視訊</a><br/><br/><a href="/windows/uwp/audio-video-camera/capture-photos-and-video-with-mediacapture">使用 MediaCapture 擷取相片和視訊</a></td>
</tr>
<tr class="even">
<td align="left"><strong>播放媒體。</strong> <br><br>播放音訊和視訊檔案。</td>
<td align="left"><strong>MediaPlayer</strong> 和 <strong>AudioManager</strong> 類別可用來播放音訊和視訊檔案。</td>
<td align="left"><strong>AVKit 架構</strong>、<strong>AVAudioPlayer</strong> 及 <strong>Media Player Framework (媒體播放程式架構)</strong> 可用來播放音訊和視訊檔案。</td>
<td align="left">您可以使用 <strong><a href="/uwp/api/Windows.Media.Core.MediaSource">MediaSource 類別</a></strong>、<strong><a href="/uwp/api/windows.ui.xaml.controls.mediaelement">MediaElement</a></strong> 及 <strong><a href="/uwp/api/windows.media.playback.mediaplayer">MediaPlayer</a></strong> 類別，播放本機和遠端檔案等來源的音訊與視訊。<br/><br/><a href="/windows/uwp/audio-video-camera/media-playback-with-mediasource">使用 MediaSource 進行媒體播放</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>正在編輯媒體。</strong> <br><br>從現有的錄製組成新的媒體檔案並套用特殊效果。</td>
<td align="left"><strong>MediaCodec</strong>、<strong>MediaMuxer</strong> 及 <strong>android.media.effect</strong> 等低階類別可用來編輯內容。</td>
<td align="left"><strong>AV Foundation</strong> 架構中的類別 (例如 <strong>AVMutableComposition</strong>、<strong>AVMutableVideoComposition</strong> 及 <strong>AVMutableAudioMix</strong>) 可用來編輯內容。</td>
<td align="left">您可以使用<strong><a href="/uwp/api/windows.media.editing.mediacomposition">MediaComposition</a></strong>和<strong><a href="/uwp/api/windows.media.editing.mediaclip">MediaClip</a></strong>之類的<strong><a href="/uwp/api/windows.media.editing">編輯</a></strong>api，從音訊和影片檔案建立媒體組合。 您可以新增視訊與影像重疊、結合視訊剪輯、新增背景音訊，以及套用音訊與視訊效果。<br/><br/><a href="/windows/uwp/audio-video-camera/media-compositions-and-editing">媒體組合和編輯</a></td>
</tr>
</tbody>
</table>
<h2 id="sensors">感應器</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>感應器。</strong> <br><br>偵測裝置的移動、位置與環境屬性。</td>
<td align="left"><strong>感應器架構</strong> 是用來搭配 <strong>SensorManager</strong> 和 <strong>SensorEvent</strong> 等類別存取硬體與軟體感應器。</td>
<td align="left"><strong>Core Motion (核心動作) 架構</strong>是用來存取原始和已處理的感應器資料。</td>
<td align="left">您可以使用 <strong><a href="/uwp/api/windows.devices.sensors">Windows</a></strong> 中的類別，來存取從感應器收到新的讀取資料時所觸發的感應器讀數和事件。<br/><br/><a href="/windows/uwp/devices-sensors/sensors">感應器</a></td>
</tr>
</tbody>
</table>
<h2 id="location-and-mapping">位置與地圖功能</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>位置。</strong> <br><br>尋找裝置的<strong>目前</strong>位置和追蹤<strong>變更</strong>。</td>
<td align="left">Google Play 服務位置 API 使用 <strong>getLastLocation</strong> 和 <strong>requestLocationUpdates</strong> 方法搭配<strong>聯合的定位提供者</strong>，提供<strong>最後已知的位置</strong>的高階存取權。 低階存取權是透過 Android 程式庫中的 <strong>LocationManager</strong> 提供。</td>
<td align="left"><strong>Core Location</strong> <strong>CLLocationManager</strong> 類別是用來監視裝置的位置 (搭配 <strong>startUpdatingLocation</strong> 用於標準位置服務；搭配 <strong>startMonitoringSignificantLocationChanges</strong> 用於<strong>重大變更</strong>位置服務)。</td>
<td align="left">您可以使用 <strong><a href="/uwp/api/windows.devices.geolocation">Windows</a></strong>中的類別來追蹤裝置位置。 針對單次讀取使用 <strong><a href="/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync">Geolocator.GetGeopositionAsync</a></strong>。 使用 <strong><a href="/uwp/api/windows.devices.geolocation.geolocator.positionchanged">Geolocator PositionChanged</a></strong> ，以定期使用計時器取得位置，或在位置變更時通知您。<br/><br/><a href="/windows/uwp/maps-and-location/get-location">取得使用者的位置</a></td>
</tr>
<tr class="even">
<td align="left"><strong>顯示對應。</strong> <br><br>顯示<strong>互動式內建地圖</strong>及新增<strong>興趣點</strong>。</td>
<td align="left"><strong>Google Maps Android API</strong> 內的 <strong>GoogleMap</strong>、<strong>MapFragment</strong> 和 <strong>MapView</strong> 類別可讓您將地圖內嵌到 app。 使用<strong>標記</strong>和可自訂的 <strong>Marker</strong> 類別可顯示興趣點。</td>
<td align="left">使用 <strong>MapKit 架構</strong>中的 <strong>MKMapView</strong> 類別，可將地圖內嵌到 iOS app。 使用物件類別 (例如 <strong>MKPointAnnotation</strong>) 和檢視類別 (例如 <strong>MKPinAnnotationView</strong>)，可將<strong>註解</strong>新增到 App 以顯示興趣點。</td>
<td align="left">您可以使用內建的 <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mapcontrol">隨 mapcontrol</a></strong> XAML 控制項（提供2D、3d 和 streetside 視圖），在您的應用程式中內嵌地圖。 您可以使用 <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mapicon">MapIcon</a></strong>、 <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mappolygon">MapPolygon</a></strong> 和 <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mappolyline">MapPolyline</a></strong>等類別，以圖釘、影像或圖形來新增感興趣的點。<br/><br/><a href="/windows/uwp/maps-and-location/display-maps">顯示地圖的 2D、3D 和 Streetside 檢視</a><br/><br/><a href="/windows/uwp/maps-and-location/display-poi">在地圖上顯示興趣點 (POI)</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>地理柵欄.</strong> <br><br>監視進入和離開的特定地理區域。</td>
<td align="left">使用 Google Play 服務 SDK 中的<strong>定位服務</strong>來監視地理柵欄。</td>
<td align="left">使用 <strong>CLCircularRegion</strong> 類別監視區域，並使用 <strong>CLLocationManager.startMonitoringForRegion:</strong> 登錄區域。</td>
<td align="left">您可以使用 <strong><a href="/uwp/api/windows.devices.geolocation.geofencing.geofence">Geofence</a></strong> 類別建立地理柵欄並定義<strong>受監視狀態</strong> (例如進入或離開某個區域)。 使用 <strong><a href="/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor">GeofenceMonitor 類別</a></strong>在前景處理地理柵欄事件，使用 <strong><a href="/uwp/api/windows.applicationmodel.background.locationtrigger">LocationTrigger 背景類別</a></strong>在背景處理地理柵欄事件。<br/><br/><a href="/windows/uwp/maps-and-location/set-up-a-geofence">設定地理柵欄</a></td>
</tr>
<tr class="even">
<td align="left"><strong>地理編碼和反向地理編碼。</strong> <br><br>將地址轉換成地理位置 (地理編碼)，以及將地理位置轉換成地址(反向地理編碼)。<br/></td>
<td align="left"><strong>Geocoder</strong> 類別用於地理編碼與反向地理編碼。</td>
<td align="left"><strong>CLGeocoder</strong> 類別用於地理編碼。</td>
<td align="left">您可以使用<strong><a href="/uwp/api/windows.services.maps">Windows</a></strong>中的<strong><a href="/uwp/api/windows.services.maps.maplocationfinder">MapLocationFinder 類別</a></strong>來執行地理編碼。 您使用 <strong><a href="/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync">FindLocationsAsync</a></strong> 執行地理編碼，並使用 <strong><a href="/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync">FindLocationsAtAsync</a></strong> 執行反向地理編碼。<br/><br/><a href="/windows/uwp/maps-and-location/geocoding">執行地理編碼和反向地理編碼</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>路線和方向。</strong> <br><br>提供兩個地理位置之間的路線、距離與路線指引。</td>
<td align="left">Google 提供 Web 服務 <strong>Google Maps Directions API</strong>，它可在 Android 上使用，雖然未提供 SDK。</td>
<td align="left">地圖套件提供 <strong>MKDirections</strong> API，可用來取得路線和路線指引的相關資訊。</td>
<td align="left">您可以使用 <strong><a href="/uwp/api/windows.services.maps.maproutefinder">MapRouteFinder</a></strong> 類別，在 <strong><a href="/uwp/api/windows.services.maps">Windows. Maps</a></strong>中要求「行走」或「駕駛」路由。 路線會以 <strong><a href="/uwp/api/windows.services.maps.maproute">MapRoute</a></strong> 執行個體傳回，在 MapControl 上可輕鬆地顯示它。 <strong><a href="/uwp/api/windows.services.maps.maproutemaneuver">MapRouteManeuver</a></strong>物件內會傳回方向。<br/><br/><a href="/windows/uwp/maps-and-location/routes-and-directions">在地圖上顯示路線和路線指引</a></td>
</tr>
</tbody>
</table>
<h2 id="app-to-app-communication">應用程式間通訊</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>叫用另一個應用程式。</strong> <br><br>啟動另一個 app，以及選擇性地分享資料 (例如，連結、文字、相片、影片及檔案)。</td>
<td align="left"><strong>隱含意圖</strong> 是用來啟動另一個 app，方法是定義 <strong>Intent (意圖)</strong> 中的<strong>動作</strong>和選擇性資料，並使用 <strong>startActivityForResult</strong> 呼叫它。<br/></td>
<td align="left"><strong>應用程式延伸</strong> 模組可用來提供應用程式資料存取給另一個應用程式。 <strong>URL 配置</strong>可讓您將 URL 傳遞給另一個 App。</td>
<td align="left">您可以啟動另一個應用程式，該應用程式已向 <strong><a href="/uwp/api/windows.system.launcher.launchuriasync">LaunchUriAsync</a></strong>或 <strong><a href="/uwp/api/windows.system.launcher.launchuriforresultsasync">LaunchUriForResultsAsync</a></strong> 註冊 URI，以啟動結果並從已啟動的應用程式取回資料。 您可以使用 <strong><a href="/uwp/api/windows.system.launcher.launchfileasync">Launcher.LaunchFileAsync</a></strong> 將檔案傳送到另一個 app 進行處理。<br/><br/>您可以使用<strong>分享協定</strong>輕鬆在 app 之間分享資料。<br/><br/><a href="/windows/uwp/launch-resume/launch-default-app">啟動 URI 的預設應用程式</a><br/><br/><a href="/windows/uwp/launch-resume/how-to-launch-an-app-for-results">啟動應用程式以取得結果</a><br/><br/><a href="/windows/uwp/launch-resume/launch-the-default-app-for-a-file">啟動檔案的預設應用程式</a><br/><br/><a href="/windows/uwp/app-to-app/share-data">共用資料</a></td>
</tr>
<tr class="even">
<td align="left"><strong>允許叫用您的應用程式。</strong> <br><br>允許您的 app 回應另一個 app 的要求。</td>
<td align="left">App 使用<strong>意圖篩選器</strong>登錄<strong>意圖處理活動</strong>，以回應另一個 app 的隱含意圖。</td>
<td align="left">封裝 <strong>app 延伸模組</strong>可讓資料與其他 app 分享。 App 可以使用 Info.plist 中的 <strong>CFBundleURLTypes</strong> 機碼來登錄<strong>自訂 URL 配置</strong>。</td>
<td align="left">您可以登錄 app 做為 <strong>URI 配置名稱</strong>的預設處理常式，方法是在套件資訊清單中登錄<strong><a href="/uwp/api/windows.applicationmodel.activation.activationkind#Protocol">通訊協定</a></strong>並更新 <strong><a href="/uwp/api/windows.ui.xaml.application.onactivated">Application.OnActivated</a></strong> 事件處理常式，也可以選擇性傳回結果。 同樣地，您可以藉由在封裝資訊清單中加入宣告並處理 <strong><a href="/uwp/api/windows.ui.xaml.application.onfileactivated">OnFileActivated</a></strong> 事件，將應用程式註冊為特定檔案類型的預設處理常式。<br/><br/>您可以處理分享協定要求，方法是在資訊清單中將 app 登錄為分享目標並處理 <strong><a href="/uwp/api/windows.ui.xaml.application.onsharetargetactivated">Application.OnShareTargetActivated</a></strong> 事件。<br/><br/><a href="/windows/uwp/launch-resume/how-to-launch-an-app-for-results">啟動應用程式以取得結果</a><br/><br/><a href="/windows/uwp/launch-resume/handle-file-activation">處理檔案啟用</a><br/><br/><a href="/windows/uwp/app-to-app/receive-data">接收資料</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>複製並貼上。</strong> <br><br>在 app 之間複製和貼上文字與其他內容。</td>
<td align="left"><strong>剪貼簿架構</strong>可讓您使用 <strong>ClipboardManager</strong> 和 <strong>ClipData</strong> 類別來實作複製和貼上。</td>
<td align="left"><strong>UIPasteboard</strong>、<strong>UIMenuController</strong> 與 <strong>UIResponderStandardEditActions</strong> 可以用來實作複製和貼上。</td>
<td align="left">許多預設 XAML 控制項已經支援複製和貼上。 使用 <strong><a href="/uwp/api/Windows.ApplicationModel.DataTransfer">Windows.ApplicationModel.DataTransfer</a></strong> 中的 <strong><a href="/uwp/api/windows.applicationmodel.datatransfer.datapackage">DataPackage</a></strong> 和 <strong><a href="/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard">Clipboard</a></strong> 類別，即可自行實作複製和貼上。<br/><br/><a href="/windows/uwp/app-to-app/copy-and-paste">複製和貼上</a></td>
</tr>
<tr class="even">
<td align="left"><strong>拖放。</strong> <br><br>在 app 之間拖放內容。</td>
<td align="left">使用 <strong>Android 拖放架構</strong>即可在單一 app 內部實作拖放功能。</td>
<td align="left">iOS 沒有提供高階的拖放功能 API。</td>
<td align="left">您可以在 app 中實作拖放功能，以啟用 app 到 app、桌面到 app 及 app 到桌面之間的拖放功能。 您可以使用 <strong><a href="/uwp/api/windows.ui.xaml.uielement.allowdrop">system.windows.uielement.allowdrop</a></strong>、 <strong><a href="/uwp/api/windows.ui.xaml.uielement.candrag">CanDrag</a></strong> 屬性、 <strong><a href="/uwp/api/windows.ui.xaml.uielement.dragover">system.windows.dragdrop.dragover></a></strong>和 <strong><a href="/uwp/api/windows.ui.xaml.uielement.drop">drop</a></strong> 事件，在 UIElement 類別中執行拖放支援。<br/><br/><a href="/windows/uwp/app-to-app/drag-and-drop">拖放功能</a></td>
</tr>
</tbody>
</table>
<h2 id="software-design">軟體設計</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>一般概念</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10 UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>軟體設計模式。</strong> <br><br>適用於平台的建議模式或常用模式。</td>
<td align="left">對於 Android 開發沒有建議或提供官方模式，不過搶鮮版 (Beta) 的「資料繫結架構」可能會讓 <strong>Model-View-ViewModel (MVVM)</strong> 模式更廣泛地使用。 許多協力廠商文章和架構建議 <strong>Model-View-Presenter (MVP)</strong> 和 <strong>MVVM</strong> 方法。</td>
<td align="left"><strong>模型-視圖-控制器 (MVC) </strong> 是與 iOS 搭配使用的常見模式，並已整合至平臺。</td>
<td align="left">針對 UWP 進行建置時，並沒有限制您必須使用特定模式。<br/><br/>您可以使用內建<a href="/windows/uwp/data-binding/index">資料繫結</a>模式以確保清楚區分資料和 UI 的考量，而且無需撰寫稍後會更新屬性值之 UI 事件處理常式的程式碼。<br/><br/>您可以延伸資料繫結以遵循 <strong>Model-View-ViewModel (MVVM)</strong> 模式，方法是使用第三方 MVVM 程式庫 (例如 <a href="https://archive.codeplex.com/?p=mvvmlight">MVVM Light Toolkit</a>)，或是啟動您自己的而不使用程式碼後置的邏輯。<br/><br/><a href="/previous-versions/msp-n-p/hh848246(v=pandp.10)">MVVM 模式</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">Template 10 Visual Studio 專案範本</a></td>
</tr>
</tbody>
</table>