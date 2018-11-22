---
author: QuinnRadich
title: Windows 10 中適合開發人員的新功能、工具與特色
description: Windows 10 組建 17134 與新的開發人員工具提供由通用 Windows 平台所提供的工具、功能及體驗。
keywords: 新功能, 最新動向, 更新, 多項更新, 功能, 新, Windows 10, 最新, 開發人員, 17134
ms.author: quradic
ms.date: 4/10/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7b25df0a8445322ceb422fd6485f7d08c1c49a25
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7567141"
---
# <a name="whats-new-in-windows-10-for-developers-build-17134"></a>適用於開發人員的 Windows 10 (組建 17134) 的新功能

Windows 10 組建 17134 (也稱為 4 月更新或版本 1803) 搭配 Visual Studio 2017 與更新的 SDK，提供工具、功能以及體驗來造就不凡的通用 Windows 平台應用程式。 在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

這是此版本中 Windows 開發人員會感興趣的新功能和改良功能以及指導方針的集合。 如需新增到 Windows SDK 之新命名空間的完整清單，請參閱 [Windows 10 組建 17134 API 變更](windows-10-build-17134-api-diff.md)。 如需 Windows 10 重點功能的詳細資訊，請參閱 [Windows10 中有哪些酷功能](http://go.microsoft.com/fwlink/?LinkId=823181)。 此外，請參閱 [Windows 開發人員平台功能](https://developer.microsoft.com/windows/platform/features)以取得過去與未來加入 Windows 平台功能的高階概觀。

## <a name="design--ui"></a>設計與 UI

功能 | 說明
 :------ | :------
調適型和互動式快顯通知 | 使用調適型和互動式通知增強您的 App 開始使用我們[更新的快顯通知相關指引](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)，並探索有關影像大小限制、進度列以及新增輸入選項的新資訊。<br><br>[ExpirationTime](https://docs.microsoft.com/uwp/api/windows.ui.notifications.scheduledtoastnotification.expirationtime#Windows_UI_Notifications_ScheduledToastNotification_ExpirationTime) 現可在排程的快顯通知上支援。
內容連結 | 新的[內容連結](../design/controls-and-patterns/content-links.md)控制項提供可在文字控制項中嵌入豐富資料的方法，讓使用者不離開應用程式的內容也能尋找和使用有關個人或位置的詳細資訊。
設計範例 | [設計工具組與範例](../design/downloads/index.md)頁面已新增 BuildCast 範例。 BuildCast 是端對端範例，用來展示 Fluent Design 系統，以及通用 Windows 平台的其他功能。
內嵌手寫 | [文字控制項](../design/controls-and-patterns/text-controls.md)已新增手寫筆輸入功能，可讓使用者使用 Windows Ink 直接寫入文字方塊。 當使用者書寫時，文字會轉換為保持自然書寫風格的書寫體文字。
Fluent Design 更新 | 我們使用新的資訊及指引，更新了許多 Fluent Design 頁面： </br> * [Fluent Design 概觀](../design/fluent-design-system/index.md)已更新，以便與最新 Fluent 功能保持一致。 </br> * [顯色醒目提示](../design/style/reveal.md)已提供有關佈景主題及自訂控制項的新指引。 </br> * [瀏覽歷程記錄與向後瀏覽](../design/basics/navigation-history-and-backwards-navigation.md)已修訂，並提供詳細範例、裝置最佳化指引以及自訂行為指引。
焦點瀏覽 | 新的[焦點瀏覽](../design/input/focus-navigation.md)主題描述如何為倚賴非指向輸入類型 (像是鍵盤、遊戲台或遙控器) 的使用者最佳化 UWP 應用程式。 此外，[程式設計焦點瀏覽](../design/input/focus-navigation-programmatic.md)描述可用來強化這些體驗的 API。
鍵盤快速鍵 | 有關[鍵盤快速鍵](../design/input/keyboard-accelerators.md)的指導方針已更新，納入了新的可用性資訊。 新增工具提示到您的鍵盤快速鍵，以及新增標籤置您的控制項，可改善探索能力，或以新的 API 覆寫預設的鍵盤快速鍵行為。
頁面配置 | 我們已使用有關流暢版面配置及視覺狀態的新資訊來更新 [XAML 頁面配置](../design/layout/layouts-with-xaml.md)文件。 這些功能允許進一步控制 App 中元素位置的回應方式，並配合可用視覺空間進行調整。
拖動以重新整理 | [拖動以重新整理](../design/controls-and-patterns/pull-to-refresh.md)控制項可讓使用者將資料清單向下拖動來擷取更多資料。 這已在有觸控式螢幕的裝置上廣泛使用。
瀏覽檢視 | [瀏覽檢視](../design/controls-and-patterns/navigationview.md)控制項在 App 中提供最上層瀏覽的可摺疊瀏覽功能表。 此控制項實作瀏覽窗格 (或漢堡式功能表) 模式，並自動配合窗格的顯示模式調整為不同視窗大小。
顯色焦點 | 新的[顯色焦點](../design/style/reveal-focus.md)效果提供體驗 (例如 Xbox One 和電視螢幕) 的光源效果。 這會在使用者將遊戲台或鍵盤焦點移近可設定焦點元素 (例如按鈕) 時，以動畫方式呈現這些元素的框線。
音效 | XAML 現在使用 **SpatialAudioMode** 屬性支援 3D 音訊。 如需其設定方式的詳細資訊，請參閱[音效](../design/style/sound.md)。
磚 | [可追蹤式磚通知](../design/shell/tiles-and-notifications/chaseable-tile-notifications.md)現可在 JavaScript 為主的 UWP app 中支援。<br><br>次要磚和徽章通知是[從傳統型橋接器應用程式現在支援](../design/shell/tiles-and-notifications/secondary-tiles-desktop-pinning.md#send-tile-notifications)。
樹狀目錄檢視 | [TreeView](../design/controls-and-patterns/tree-view.md) 控制項啟用提供包含巢狀項目的展開及摺疊節點的階層式清單。 此控制項可以用來說明 UI 中的資料夾結構或巢狀關聯性。
撰寫方式 | 我們已針對有關語音與音調的文章提升品質並擴充內容，並將其轉換成[撰寫方式指引](../design/style/writing-style.md)。 這項新資訊提供關於在 App 中建立有效文字的原則，並建議撰寫控制項 (例如錯誤訊息或對話方塊) 的最佳做法。

## <a name="gaming"></a>遊戲
功能 | 描述
 :------ | :------
遊戲開發入門 | 對開發 Windows 10 遊戲感興趣？ 新的[遊戲開發入門](../gaming/getting-started.md)頁面提供您自行完成設定、註冊，以及準備好提交應用程式與遊戲所需執行之動作的完整概觀。
圖形卡 | 已新增與圖形卡喜好設定及移除作業相關的下列 DXGI API： </br> * [IDXGIFactory6](https://msdn.microsoft.com/library/windows/desktop/mt814823) 介面啟用根據指定之 GPU 喜好設定列舉圖形卡的單一方法。 </br> * [DXGIDeclareAdapterRemovalSupport](https://msdn.microsoft.com/library/windows/desktop/mt814821) 函式可讓處理序指出其任何即將移除的圖形裝置可復原。 </br> * [DXGI_GPU_PREFERENCE](https://msdn.microsoft.com/library/windows/desktop/mt814822) 列舉描述 App 用來執行的 GPU 喜好設定。

## <a name="develop-windows-apps"></a>開發 Windows 應用程式

功能 | 描述
 :------ | :------
調整卡 | [調整卡](https://docs.microsoft.com/adaptive-cards/)是開放式卡片交換格式，可讓開發人員透過通用且一致的方式交換 UI 內容。 這些卡片將其內容描述為 JSON 物件，這些物件可進行轉譯以自動配合主應用程式的外觀及風格做調整。
應用程式資源群組 | [AppResourceGroupInfo](https://docs.microsoft.com/uwp/api/windows.system.appresourcegroupinfo) 類別有新的方法可以用來起始應用程式暫停、使用中 (已繼續) 及終止狀態的轉換。
廣泛的檔案系統存取 | **broadFileSystemAccess** 功能在沒有檔案選擇器樣式提示的情況下，將目前正在執行應用程式的使用者同樣的檔案系統存取權授與應用程式。 如需詳細資訊，請參閱[檔案存取權限](../files/file-access-permissions.md)以及 [App 功能宣告](../packaging/app-capability-declarations.md) 中的 **broadFileSystemAccess** 項目。
C++/WinRT | [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) 是 Windows 執行階段 (WinRT) API 的全新完全標準現代 C++17 語言投影。 它僅在標頭檔案中實作，以及設計用來提供您現代化 Windows API 的第一級存取。 使用 C++/WinRT，您可以撰寫及取用使用任何符合標準 C++17 編譯器的 WinRT API。 針對您的 C++ 應用程式 — 從 Win32 到 UWP — 使用 C++/WinRT 讓您的程式碼保持標準、現代化且清楚明瞭，同時讓應用程式保持輕量且快速。
主機 UWP app | 您現在可以撰寫執行於主控台視窗 (例如 DOS 或 PowerShell 主控台視窗) 中的 C++ /WinRT 或 /CX UWP 主控台應用程式。 主控台應用程式使用主控台視窗進行輸入和輸出。 UWP 主控台應用程式可以發佈至 Microsoft Store，列入應用程式清單，以及列入可釘選到 [開始] 功能表的主要磚。 如需詳細資訊，請參閱[建立通用 Windows 平台主控台應用程式](../launch-resume/console-uwp.md)
更多應用程式資訊清單功能 | 應用程式套件資訊清單結構描述中新增了幾項功能，包括：廣泛的檔案系統存取、針對服務點裝置啟用條碼掃描器、定義 UWP 主控台應用程式等。 如需詳細資料，請參閱 [Windows 10 中的應用程式資訊清單變更](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/what-s-changed-in-windows-10)。
無障礙技術 (AT) 支援的地標和標題 | 地標和標題定義可協助輔助技術 (例如螢幕助讀程式) 使用者有效率進行瀏覽的使用者介面區段。 如需詳細資訊，請參閱[地標和標題](../design/accessibility/landmarks-and-headings.md)。
機器學習 | Windows Machine Learning 可讓您建置透過 Windows 10 裝置本機預先訓練機器學習模型進行問題求解的應用程式。 若要深入了解此平台，請參閱 [Windows Machine Learning](https://docs.microsoft.com/windows/ai/)。 </br> [MachineLearning](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.preview) 命名空間包含可讓應用程式載入機器學習模型、繫結資料做為輸入以及評估結果的類別。
地圖控制項 | [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) 類別有名為 **Region** 的新屬性，可以用來根據特定地區 (例如，縣市或省份) 的語言的顯示地圖控制項內容。
地圖元素 | [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 類別有名為 **IsEnabled** 的新屬性，可以用來指定使用者可否與 [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 進行互動。
地圖地點資訊 | [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo) 類別包含新方法 **CreateFromAddress**，可以用來使用地址和顯示名稱建立 [PlaceInfo](https://docs.microsoft.com/uwp/api/windows.services.maps.placeinfo)。
地圖服務 | [MapRouteDrivingOptions](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutedrivingoptions) 類別包含名為 **DepartureTime** 的新屬性，可以用來透過指定之日期與時間的一般交通狀態計算路線。
多執行個體 UWP app | UWP app 可以選擇支援多個執行個體。 如果多執行個體 UWP app 的其中一個執行個體正在執行，而且後續啟用要求成功時，平台將不會啟用現有的執行個體。 反而會建立執行於不同處理序中的新執行個體。 如需詳細資訊，請參閱[建立多執行個體通用 Windows app](../launch-resume/multi-instance-uwp.md)
套件資源索引 API 和自訂組建系統 | 您可以使用[套件資源索引 (PRI) API](https://docs.microsoft.com/windows/uwp/app-resources/pri-apis-custom-build-systems)，開發適用於您的 UWP app 資源的自訂組建系統。 建置系統可以依據 UWP app 所需的任何複雜層級，建立 PRI 檔案並對這些檔案進行版本控制和傾印。 如果您的自訂建置系統目前使用 MakePri.exe 命令列工具，建議您改為呼叫 PRI API，因為它們提供更高的效能與控制。
PlayReady | Microsoft PlayReady 是一組保護數位內容免遭未經授權使用的技術。 PlayReady 可在各種裝置和 App 上執行，以及跨所有作業系統執行。 [了解如何將 PlayReady 納入您的 App 中。](https://docs.microsoft.com/playready/)
私人對象 | 如果您只想將您的應用程式 Store 清單顯示給您指定的特定人員，請使用新的 **\[私人對象\]** 選項。 除了您指定群組中的人員，其他人都無法找到或使用此應用程式。 這個選項適用於 Beta 測試，因為您可以將應用程式散發給測試人員，而其他任何人都無法取得應用程式，甚至也無法查看其 Store 清單。 如需詳細資訊，請參閱[選擇可見度選項](../publish/choose-visibility-options.md)。
漸進式 Web 應用程式 | Microsoft Edge 和 UWP Web app 現在支援[漸進式 Web 應用程式 (PWA)](https://docs.microsoft.com/microsoft-edge/progressive-web-apps)! </br> * 使用[標準 Web 技術](https://developer.mozilla.org/Apps/Progressive)和[功能偵測](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)，可以增強 Web 應用程式以提供原生應用程式體驗，包括推播通知、離線支援和 OS 整合，同時在瀏覽器和尚不支援 PWA 技術的平台上仍提供絕佳的基準 Web 應用程式體驗。 </br> * [新增資訊清單檔案](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)到您的 app 可讓它安裝到整個 UWP 裝置系列 (包括安全的 [Windows 10 S 模式裝置](https://www.microsoft.com/windows/windows-10-s))，並且從 [Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store) 散發。 </br> PWA 是*託管的 Web 應用程式*的自然進化版，但擁有離線案例的標準支援，全都拜*服務程式*、*快取*和*推播 API* 之賜。
螢幕擷取 | [Windows.Graphics.Capture 命名空間](https://docs.microsoft.com/uwp/api/windows.graphics.capture)提供 API，從顯示畫面或應用程式視窗取得畫面格來建立要建置共同作業及互動體驗的視訊串流或快照。 如需詳細資訊，請參閱 [螢幕擷取](../audio-video-camera/screen-capture.md)。
系統觸發程序 | [CustomSystemEventTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.customsystemeventtrigger) 可讓您在作業系統未提供您需要的系統觸發程序時定義系統觸發程序。 例如，當硬體驅動程式和 UWP app 都屬於協力廠商，而且硬體驅動程式必須引發其 App 處理的自訂事件時。 例如，需要通知使用者關於何時插入音訊插孔時的音訊卡。
使用者活動 | 新的 [UserActivity 文件](../launch-resume/useractivities.md) 說明如何協助使用者繼續使用您的應用程式執行作業，甚至是在多個裝置之間作業。</br>**UserActivitySessionHistoryItem** 類別有擷取最近使用者活動的新方法。 如需詳細資訊，請參閱 [GetRecentUserActivitiesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel.getrecentuseractivitiesasync) 及其多載。
Windows Mixed Reality API | 為了支援逐漸茁壯的 Windows Mixed Reality 平台，新的 API 已新增至 [Windows.Graphic.Holographic](https://docs.microsoft.com/uwp/api/Windows.Graphics.Holographic) 和 [Windows.UI.Input.Spatial](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Spatial) 命名空間。
Windows Mixed Reality 文件 | Windows Mixed Reality 開發人員指導方針[現在 docs.microsoft.com 上裝載。](https://docs.microsoft.com/windows/mixed-reality/) 就像在這些 UWP 文件中，您可以現在檔案 GitHub 問題的意見反應或送出您自己的貢獻，透過提取要求。

## <a name="publish--monetize-windows-apps"></a>發佈 Windows 應用程式以及從中獲利

功能 | 描述
 :------ | :------
從 Microsoft Store 下載與安裝套件更新 | 我們已更新[從 Microsoft Store 下載與安裝套件更新](../packaging/self-install-package-updates.md)並提供新的指引和範例，說明如何下載和安裝套件更新，而不需對使用者顯示通知 UI、解除安裝選用套件，以及取得 app 的下載和安裝佇列中有關套件的資訊。
輸入以特定市場當地貨幣表示的自由格式價格 | 當您覆寫 App 的特定市場基本價格時，不再受到只能選擇其中一個標準價格區間的限制。您現在可以選擇輸入以特定市場當地貨幣表示的自由格式價格。 如需詳細資訊，請參閱[設定與排程應用程價格](../publish/set-and-schedule-app-pricing.md)。 **所有 Windows 開發人員都能使用這項功能，不需要更新 SDK。**
存放區內容 | [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 類別經過更新，提供一組精選的新方法。 這些方法會管理應用程式套件更新及附加元件的下載與安裝。
訂閱附加元件現在可提供給所有開發人員 | 建立和發佈訂閱附加元件，透過自動化固定計費週期在您的 App 和遊戲中銷售數位產品 (例如應用程式功能或數位內容)。 如需詳細資訊，請參閱[啟用應用程式的訂閱附加元件](../monetize/enable-subscription-add-ons-for-your-app.md)。 **所有 Windows 開發人員都能使用這項功能，不需要更新 SDK。**

## <a name="videos"></a>影片

下列影片自 Fall Creators Update 發行後即已發佈，重點說明 Windows 10 中適用於開發人員的新功能及改良功能。

### <a name="accessibility-tools-for-windows-developers"></a>適用於 Windows 開發人員的協助工具

Windows 10 SDK 包含數個工具來幫助您測試和改善 App 的協助工具。 檢查工具和 AccEvent 工具協助您確保 App 可供所有使用者使用。 [觀看影片](https://www.youtube.com/watch?v=ce0hKQfY9B8&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF&t=0s&index=1)了解這些工具，然後[閱讀更多有關協助工具測試的資訊](../design/accessibility/accessibility-testing.md)以取得詳細資訊。

### <a name="creating-3d-app-launchers-for-windows-mixed-reality"></a>建立 Windows Mixed Reality 的 3D 應用程式啟動器

3D 啟動器提供獨特方式，讓使用者將您的 App 的真實體積測量表示法放置在混合實境主要環境中。 [觀看影片](https://www.youtube.com/watch?v=TxIslHsEXno)了解如何準備 3D 模型，並指派其做為您的 App 的啟動器，然後[閱讀開發人員文件](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_app_launchers)和[查看我們的設計指引](https://developer.microsoft.com/windows/mixed-reality/3d_app_launcher_design_guidance)以取得詳細資訊。

### <a name="creating-a-uwp-console-app"></a>建立 UWP 主控台應用程式

您現在可以建立在 PowerShell 或 DOS 主控台視窗內執行的 UWP app。 [觀看影片](https://www.youtube.com/watch?v=bwvfrguY20s&t=0s&index=1&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF)了解怎麼做，然後[查看文件](../launch-resume/console-uwp.md)取得詳細資訊。 

### <a name="how-to-use-windows-ml-in-your-app"></a>如何在您的應用程式中使用 Windows ML

Windows Machine Learning 可讓您建置透過 Windows 10 裝置本機預先訓練機器學習模型進行問題求解的應用程式。 [觀看影片](https://www.youtube.com/watch?v=8MCDSlm326U&index=2&list=PLWs4_NfqMtoycBFndriDmkQlMLwflyoFF)取得快速逐步說明，然後[閱讀文件](https://docs.microsoft.com/windows/uwp/machine-learning/)取得完整資訊。

### <a name="motion-controller-tracking"></a>運動控制器追蹤

運動控制器在 Windows Mixed Reality 中代表使用者的雙手。 [觀看影片](https://www.youtube.com/watch?v=rkDpRllbLII)了解運動控制器處於混合實境頭戴式裝置視野範圍內外的運作方式，並[在這裡閱讀更多有關控制器追蹤的資訊](https://developer.microsoft.com/windows/mixed-reality/motion_controllers#controller_tracking_state%E2%80%9D)。

### <a name="package-a-net-app-in-visual-studio"></a>在 Visual Studio 中封裝 .NET 應用程式

比以往更輕鬆地將您的傳統型應用程式攜入通用 Windows 平台。 [觀賞影片](https://www.youtube.com/watch?v=fJkbYPyd08w)以了解如何封裝您的 .NET 應用程式以供發佈，接著[查看這個頁面](../porting/desktop-to-uwp-packaging-dot-net.md)以取得詳細資訊。

### <a name="xbox-live-creators-program"></a>Xbox Live 創作者計畫

Xbox Live 創作者計畫可讓開發人員快速將 UWP 遊戲發行至 Xbox One 和 Windows 10。 [觀看影片](https://www.youtube.com/watch?v=zpFfHHBkVq4)深入了解此計畫，然後[瀏覽此頁面](https://www.xbox.com/developers/creators-program)開始進行。

### <a name="one-dev-question---why-was-docments-and-settings-renamed-users"></a>One Dev Question - Documents and Settings 重新命名為 Users 的原因是什麼？

想知道為什麼重新命名 Documents and Settings 目錄嗎？ [Raymond Chen 說明名稱由來，以及變更的原因](https://www.youtube.com/watch?v=4vDHQewVmM8&index=1&list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)。 如需有關 Windows 的開發詳細資料及其歷史，請查看 [Raymond 的部落格](https://blogs.msdn.microsoft.com/oldnewthing/)。


## <a name="samples"></a>範例

### <a name="coloring-book"></a>Coloring Book (著色本)

[Coloring Book (著色本) 範例](https://github.com/Microsoft/Windows-appsample-coloringbook)有了重大更新，納入了進階筆跡案例，包括使用自訂筆跡乾燥 API 改善筆跡轉譯效能。 另外還包括支援在作品定義的區域線條內填滿和著色。 

### <a name="photo-lab"></a>Photo Lab

[Photo Lab 範例](https://github.com/Microsoft/Windows-appsample-photo-lab)已更新，可在檔案量大時使用資料虛擬化從圖片媒體櫃載入影像，以提升效能。 此外，範例中的影像編輯頁面現在使用 [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) 類別套用效果。