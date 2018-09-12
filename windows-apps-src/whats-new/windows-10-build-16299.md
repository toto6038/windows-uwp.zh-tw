---
author: QuinnRadich
title: Windows 10 中適合開發人員的新功能、工具與特色
description: Windows 10 組建 16299 與新的開發人員工具提供由通用 Windows 平台所提供的工具、功能及體驗。
keywords: 新功能, 更新, 功能, 全新, Windows 10, 1709, 10 月, 最新版, 開發人員, 16299, Fall Creators
ms.author: quradic
ms.date: 11/02/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: ffe7de94e4a8564b4971fda0b64f6648d9b6088b
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2018
ms.locfileid: "3936355"
---
# <a name="whats-new-in-windows-10-for-developers-build-16299"></a>適用於開發人員的 Windows 10 (組建 16299) 的新功能

Windows 10 組建 16299 (也稱為 Fall Creators Update 或 1709 版本) 搭配 Visual Studio 2017 與更新的 SDK，提供工具、功能以及體驗來造就不凡的通用 Windows 平台應用程式。 在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

這是此版本中 Windows 開發人員會感興趣的新功能和改良功能以及指導方針的集合。 如需新增到 Windows SDK 之新命名空間的完整清單，請參閱 [Windows 10 組建 16299 API 變更](windows-10-build-16299-api-diff.md)。 如需 Windows 10 重點功能的詳細資訊，請參閱 [Windows10 中有哪些酷功能](http://go.microsoft.com/fwlink/?LinkId=823181)。 此外，請參閱 [Windows 開發人員平台功能](https://developer.microsoft.com/windows/platform/features)以取得過去與未來加入 Windows 平台功能的高階概觀。

## <a name="design--ui"></a>設計與 UI

功能 | 描述
 :------ | :------
條件式 XAML | 您現在可以使用[條件式 XAML](../debug-test-perf/conditional-xaml.md) 來建立[版本調適型應用程式](../debug-test-perf/version-adaptive-apps.md)。 條件式 XAML 可讓您在 XAML 標記中使用 **ApiInformation.IsApiContractPresent** 方法，因此可以根據 API 是否存在來設定屬性和具現化物件，而不必使用程式碼後置。
設計工具組 | [UWP app 的設計工具組和資源](../design/downloads/index.md)已藉由新增 Sketch 及 Adobe XD 工具組進行擴充。 先前既有的工具組也已更新並修訂，為您的 UWP app 提供更強固的控制項與版面配置範本。 此外，也加入新的工具和範例，以提供範例與靈感。
Fluent Design 效果 | 這些新效果屬於 Fluent Design 系統的一部分，採用深度、透視以及移動協助使用者專注於重要的 UI 項目。 </br>* [壓克力材質](../design/style/acrylic.md)是一種建立透明紋理的筆刷。 </br>* [視差效果](../design/motion/parallax.md)可為您的應用程式增加立體深度及透視效果。 </br>* [顯色](../design/style/reveal.md)會顯目提示您應用程式中的重要項目。 </br> 如需詳細資訊，請參閱 [Fluent Design 概觀](https://fluent.microsoft.com/)。
鍵盤快速操作 | 利用[鍵盤快速鍵](../design/input/keyboard-accelerators.md)或快速鍵，提升您應用程式的存取性和可用性。 這些功能提供直覺的方式，讓使用者不需瀏覽應用程式 UI 即可叫用常見的動作或命令，並可設定為符合其功能的必要範圍。
筆跡 | [CoreIncrementalInkStroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.core.coreincrementalinkstroke) API 允許使用個別 **InkPoint** 物件，建立以遞增方式呈現的個別筆墨筆劃。 </br></br> [CoreInkPresenterHost](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.core.coreinkpresenterhost) API 讓您不需相關 **InkCanvas** 控制項即可裝載 **InkPresenter** 物件。
星形控制器 | [RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration)API 已更新，可將 **RadialController** 功能表的範圍設定在應用程式的檢視或程序。
動態磚 | [從傳統型橋接器 Win32 應用程式釘選次要磚](../design/shell/tiles-and-notifications/secondary-tiles-desktop-pinning.md)。
快顯通知 | 使用按鈕的[擱置更新](../design/shell/tiles-and-notifications/toast-pending-update.md)在快顯通知建立多個步驟互動。
UI 控制項 | 這些新的控制項使得快速建立美觀 UI 的工作變得更為輕鬆。 </br>* [色彩選擇器控制項](../design/controls-and-patterns/color-picker.md)可讓使用者瀏覽及選取色彩。 </br>* [導覽檢視控制項](../design/controls-and-patterns/navigationview.md)可讓您將頂層導覽功能輕鬆的加入到您的應用程式之中。 </br>* [個人圖片控制項](../design/controls-and-patterns/person-picture.md)會為個人顯示圖片影像。 </br>* [評分控制項](../design/controls-and-patterns/rating.md)可讓使用者輕鬆的檢視及設定評分，以反映其對內容和服務的滿意程度。
語氣和語調 | 我們新增新的 [UWP app 中的語氣和語調指導方針](../design/style/voice-and-tone.md)，提供撰寫應用程式中文字的建議。 無論您要創造什麼，請切記您使用的語言必須平易近人、友善且具有資訊性。

## <a name="gaming"></a>遊戲

功能 | 描述
 :------ | :------
遊戲廣播 | **[Windows.Media.AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)** 命名空間的新 API 可讓您的應用程式啟動系統提供的遊戲廣播 UI。 </br>您也可以註冊在廣播開始或停止時通知您應用程式的事件。 **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)** 命名空間的新 API 可讓您錄製音訊和視訊以及收錄遊戲的螢幕擷取畫面。 </br>您也可以提供中繼資料，供系統嵌入至廣播及擷取串流，讓應用程式提供同步遊戲事件的檢視體驗。 如需這些功能的詳細資訊，請參閱[遊戲廣播及擷取](../gaming/game-broadcast-and-capture.md)。
遊戲聊天覆疊 | [GameChatOverlay 類別](https://docs.microsoft.com/uwp/api/windows.gaming.ui.gamechatoverlay)提供方法，可用於取得預設遊戲聊天覆疊執行個體、設定想要的覆疊位置，以及新增訊息。
遊戲裝置資訊 | 由於主控台的功能不同，通用 Windows 平台 (UWP) 遊戲開發人員需要能夠判斷遊戲執行的主控台類型，以便做出最佳利用硬體的執行階段選擇。 **&lt;gamingdeviceinformation.h&gt;** 中的[遊戲裝置資訊](https://aka.ms/gamingdeviceinfo) API 即可提供這項功能。
遊戲模式 | 通用 Windows 平台 (UWP) 的[遊戲模式](https://msdn.microsoft.com/library/windows/desktop/mt808808) API 可讓您利用 Windows 10 中的遊戲模式產生最佳化的遊戲體驗。 這些 API 位於 **&lt;expandedresources.h&gt;** 標頭檔中。
遊戲監視器 | [GameMonitor 類別](https://docs.microsoft.com/uwp/api/windows.gaming.ui.gamemonitor)允許應用程式取得裝置的遊戲監視權限狀態，並可能提示使用者啟用遊戲監視。
TruePlay | [TruePlay](https://aka.ms/trueplay) 提供一組新工具給開發人員，可讓他們打擊在其電腦遊戲中的作弊行為。 在 TruePlay 中註冊的遊戲會在受保護程序中執行，減輕一些常見的攻擊。 通用 Windows 平台 (UWP) 適用的 TruePlay API 允許遊戲及遊戲監控系統在 Windows 10 電腦上進行有限的互動。 這些 API 位於 **&lt;gamemonitor.h&gt;** 標頭中。
Xbox Live | 我們已經為 Xbox Live 開發人員新增關於 UWP 遊戲和 Xbox 開發人員套件 (XDK) 遊戲的文件。 </br>* 請參閱 [Xbox Live 開發人員指南](../xbox-live/index.md)，以了解如何使用 Xbox Live API 將您的遊戲連線至 Xbox Live 社交遊戲網路。 </br>* 任何 UWP 遊戲開發人員都可以利用 [Xbox Live 創作者計畫](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)，在電腦和 Xbox One 上開發並發行支援 Xbox Live 的遊戲。 </br>* 請參閱 [Xbox Live 開發人員計畫概觀](../xbox-live/developer-program-overview.md)，以取得提供給 Xbox Live 開發人員的程式和功能的相關資訊。

## <a name="develop-windows-apps"></a>開發 Windows 應用程式

功能 | 描述
 :------ | :------
啟動 UWP app | 現已提供下列新功能： </br>* 使用 [StartupTask 類別](https://docs.microsoft.com/uwp/api/windows.applicationmodel.startuptask)來指定當使用者登入或系統開機時啟動的 UWP app。 </br> * 識別 UWP app 是否[由命令列啟動](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)。 </br>* 使用 [RequestRestartAsync() 和 RequestRestartForUserAsync()](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication) API，以程式設計方式要求您的 UWP app 重新啟動。 </br>* [啟動 Windows 設定應用程式](../launch-resume/launch-settings-app.md)已更新，以反映新的 URI 配置，例如 `ms-settings:storagesense`、`ms-settings:cortana-notifications` 等等。
應用程式封裝 | 應用程式安裝程式已擴充為允許從網頁下載 UWP app 套件。 此外，現在也能使用應用程式安裝程式下載一組相關應用程式套件。 請參閱新的[使用應用程式安裝程式安裝 UWP app](../packaging/appinstaller-root.md) 一節，以深入了解。
應用程式服務與延伸模組 | 我們加入了新指南[建立和使用應用程式延伸模組](../launch-resume/how-to-create-an-extension.md)，可協助您撰寫和裝載通用 Windows 平台 (UWP) 應用程式延伸模組，透過使用者可從 Microsoft Store 安裝的套件來擴充應用程式。 </br></br> 我們加入了新指南[透過服務、延伸模組和套件擴充您的應用程式](../launch-resume/extend-your-app-with-services-extensions-packages.md)，將 Windows 10 中您可用來擴充及組件化應用程式的各種技術分門別類。
背景工作 | 我們加入了三份指南，可協助您充分運用背景工作：</br></br> * [在背景無限期執行](../launch-resume/run-in-the-background-indefinetly.md)可在沒有任何背景或延伸執行節流的情況下，使用裝置上所有可用的資源。 這適用於企業 UWP app 和不會提交至 Microsoft Store 的 UWP app。 </br></br> * [從您的 App 觸發背景工作](../launch-resume/trigger-background-task-from-app.md)可從您的應用程式中啟動背景工作。 </br></br>* [在更新 UWP app 時執行背景工作](../launch-resume/run-a-background-task-during-updatetask.md)可建立在您的 UWP app 更新時執行的背景工作。
Cortana | 使用 [Cortana 技能套件](https://docs.microsoft.com/cortana/skills/overview)可新增並測試技能，以擴充 Cortana 的原有功能，並讓它與您的應用程式和服務互動。
傳統型橋接器 | 我們加入了三份指南，協助您新增適用於 Windows 10 傳統型應用程式的現代化體驗： </br>* [增強您的 Windows 10 傳統型應用程式](../porting/desktop-to-uwp-enhance.md)指南可尋找和參考正確的檔案，然後撰寫程式碼來增強 UWP 體驗，讓 Windows 10 使用者眼睛為之一亮。 </br></br>* [使用現代化 UWP 元件擴充您的傳統型應用程式](../porting/desktop-to-uwp-extend.md)結合現代化 XAML UI 與其他必須在 UWP app 容器中執行的 UWP 體驗。 </br></br>* [將您的應用程式移轉到通用 Windows 平台](../porting/desktop-to-uwp-migrate.md)可在您的 WPF、Windows Forms、UWP、Android 和 iOS 應用程式之間共用程式碼。
傳統型橋接器封裝 | Visual Studio 引進了新的[封裝專案](../porting/desktop-to-uwp-packaging-dot-net.md)，讓您不必執行以往封裝完全信任傳統型應用程式過程中需要執行的所有手動步驟。 只需加入封裝專案、參考桌面專案，然後按 F5 對您的應用程式進行偵錯。 不需進行任何手動調整。 這個有效率的全新體驗可以大幅改善舊版 Visual Studio 中所提供的體驗。
診斷和執行緒處理 | 新的診斷 API 提供執行應用程式的相關資訊： </br></br>* [AppMemoryReport](https://docs.microsoft.com/uwp/api/Windows.System.AppMemoryReport) 類別提供應用程式總認可限制、私人認可使用量及其他項目的相關資訊。 </br>* [AppDiagnosticInfo](https://docs.microsoft.com/uwp/api/windows.system.appdiagnosticinfo) 類別現在可以監控應用程式或工作的執行狀態，並提供執行狀態變更時的通知。 </br>* [MemoryManager](https://docs.microsoft.com/uwp/api/windows.system.memorymanager) 類別有新的方法來設定應用程式記憶體的使用量限制並報告預期的應用程式記憶體使用量限制。 </br></br>您可以依照優先順序佇列工作，並使用 [DispatcherQueue](https://docs.microsoft.com/uwp/api/windows.system.dispatcherqueue) 類別在不同的執行緒上執行工作。 此功能也能透過 [CreateDispatcherQueueController](https://msdn.microsoft.com/library/windows/desktop/mt826210.aspx) 功能在 Win32 上使用。
EdgeHTML 16 | 為 Microsoft Edge 和 JS 型通用 Windows 平台應用程式提供動力的 Web 平台已更新至 EdgeHTML 16，並且現在包含 F12 開發人員工具的重大改進、支援 CSS 格線版面配置，以及其他數個重要功能。 </br></br> * Microsoft Edge 現在支援 [CSS 格線版面配置](https://docs.microsoft.com/microsoft-edge/dev-guide/css/grid-layout)。 格線版面配置定義二維的格線型版面配置系統，可提供比使用浮點數或指令碼定位更高的版面配置流暢度。</br></br> * [Microsoft Edge 之 F12 DevTools 文件](https://docs.microsoft.com/microsoft-edge/f12-devtools-guide)已更新，改進穩定性和效能。 此外也加入新功能，用以最佳化您的開發體驗。 </br></br>* 僅限 Microsoft Edge，[WebVR](https://docs.microsoft.com/microsoft-edge/webvr/) 已新增支援[運動控制器](https://docs.microsoft.com/microsoft-edge/webvr/input#controller-buttons)和多種 [Windows Mixed Reality 頭戴式裝置](https://docs.microsoft.com/microsoft-edge/webvr/hardware)。 WebVR 也已最佳化為支援高達每秒 90 個畫面格。 </br></br> 請參閱 [Microsoft Edge 開發人員指南](https://docs.microsoft.com/microsoft-edge/dev-guide) 以取得完整的變更和新支援 API 清單。
地圖 3D 元素 | 您可以加入 3D 物件至地圖。 您可以使用新的 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 類別，從 [3D 製造格式 (3MF)](http://3mf.io/specification/) 檔案匯入 3D 物件。
地圖元素樣式 | 您可以使用兩個新的 [MapElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement) 屬性：[MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntry) 和 [MapStyleSheetEntryState](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntryState)，自訂地圖元素的外觀。 </br></br>* 您可以使用 [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntry) 屬性，確保您的地圖元素看起來像是基底地圖的一部分 (例如：將元素樣式設定為地圖樣式表中的現有項目，例如*水*)。 </br></br>* 您可以使用 [MapStyleSheetEntryState](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.MapStyleSheetEntryState) 屬性，在地圖樣式表中運用預設狀態 (例如*暫留*和*已選取*) 修改您的地圖元素外觀，或覆寫這些元素以建立您自己的。
地圖層 | 您可以新增感興趣的地點元素至[地圖層](../maps-and-location/display-poi.md#layers)，再直接繫結 XAML 至該層。 將您的元素分組為層。 然後，您就可以獨立操作每一層。 例如，每一層都有自己的事件組，讓您可以回應特定層的特定事件，以及執行該事件專屬的動作。
地圖地點資訊 | 您可以在 UI 元素上方、下方或側面的[輕量彈出式視窗](../maps-and-location/display-maps.md#placecard)中，或使用者觸控的應用程式區域中，顯示地圖給使用者。  當使用者變更內容時，這個視窗就會自行關閉。 這讓使用者無需在應用程式或瀏覽器視窗之間切換，即可取得位置資訊。
地圖服務 | 想去觀光嗎？ 使用新的 [MapRouteOptimization.Scenic](https://docs.microsoft.com/uwp/api/windows.services.maps.maprouteoptimization) 值，可最佳化路徑以包含大部分景區道路，以及使用 [MapRoute.IsScenic](https://docs.microsoft.com/uwp/api/windows.services.maps.maproute) 來探索現有路徑是否包含景區道路。
媒體擷取 | 已更新[使用 MediaFrameReader 處理媒體畫面](../audio-video-camera/process-media-frames-with-mediaframereader.md)文章來說明如何使用新的 [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) 類別，這個類別可讓您從多個媒體來源取得與時間相互關聯的畫面。 </br></br>[使用 MediaFrameReader 處理媒體畫面](../audio-video-camera/process-media-frames-with-mediaframereader.md)已經更新以包含緩衝畫面擷取模式，可讓應用程式要求依序提供取得的畫面給應用程式，而不會在應用程式處理先前的畫面時捨棄已取得的畫面。 </br></br>此外，已使用包含一或多個媒體畫面來源的媒體畫面來源群組初始化 **MediaCapture** 物件時，您可以建立 **[MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)** 物件，讓您在 XAML 頁面的 **MediaPlayerElement** 控制項中顯示媒體畫面。</br></br>如需詳細資訊，請參閱[使用 MediaFrameReader 處理媒體畫面](../audio-video-camera/process-media-frames-with-mediaframereader.md)。
媒體播放 | 媒體播放文章[使用 MediaPlayer 播放音訊和視訊](../audio-video-camera/play-audio-and-video-with-mediaplayer.md)中加入了新章節。 </br></br>* [使用 MediaPlayer 播放球面視訊](../audio-video-camera/play-audio-and-video-with-mediaplayer.md)小節告訴您如何播放球面編碼視訊，包括針對支援的格式調整視野及觀看方向。 </br></br>* [以畫面伺服器模式使用 MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) 告訴您如何從使用 [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 播放的媒體將畫面複製到 Direct3D 表面。 這會啟用像使用像素著色器套用即時效果這樣的案例。 範例程式碼示範使用 Win2D 快速實作視訊播放模糊效果。
朋友圈 | 朋友圈可讓使用者直接從應用程式釘選連絡人到他們的工作列。 [了解如何將朋友圈支援加入您的應用程式。](../contacts-and-calendar/my-people-support.md) </br></br>* [朋友圈分享](../contacts-and-calendar/my-people-sharing.md)可讓使用者透過您的應用程式直接從工作列分享檔案。 </br>* [朋友圈通知](../contacts-and-calendar/my-people-support.md)是一種新的快顯通知，使用者可以將此通知傳送給他們釘選的連絡人。
.NET Standard 2.0 | 通用 Windows 平台已完整實作 [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support)。 這個新標準版本包含大量增加的 .NET API 以及您最愛 NuGet 套件和協力廠商程式庫的相容性填充碼。 </br></br> 如果您想以其他平台做為目標，例如 iOS 和 Android，或如果您有傳統型應用程式並想要建立 UWP app，請將程式碼移至 .NET Standard 2.0 類別庫，然後在您應用程式的每個版本中重複使用該程式碼。
釘選到工作列 | 新的 TaskbarManager 類別可讓您要求使用者[將您的應用程式釘選到工作列](../design/shell/pin-to-taskbar.md)。
服務點 | 我們加入了新的指南，可協助您[開始使用點服務裝置](../devices-sensors/pos-get-started.md)。 內容涵蓋裝置列舉、檢查裝置功能、宣告裝置和裝置共用等主題。
語音辨識 | 您現在可以使用 [SpeechRecognitionListConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) 搭配 Web 服務 [SpeechRecognitionTopicConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint)，透過提供一組您認為可能在聽寫時用到的網域特定關鍵字，來提高聽寫正確性。
使用者活動 | 新的 [Windows.ApplicationModel.UserActivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities) API 可讓您將稍後可以在不同的裝置上繼續進行的使用者工作加以封裝。

## <a name="publish--monetize-windows-apps"></a>發佈 Windows 應用程式以及從中獲利

本節中的功能在發行 Windows 1703 先前版本時已加入。 所有 Windows 開發人員都能使用這些功能，不需要更新 SDK。

功能 | 描述
 :------ | :------
帳戶管理 | [將 Azure AD 租用戶關聯至開發人員中心帳戶](../publish/associate-azure-ad-with-dev-center.md)來新增多個帳戶使用者時，我們現在提供更大彈性。 您可以將多個 Azure AD 租用戶與單一開發人員中心帳戶產生關聯，或將單一 Azure AD 租用戶與多個開發人員中心帳戶產生關聯。
廣告 | Microsoft Advertising SDK 現已可讓您在應用程式中顯示[原生廣告](../monetize/native-ads.md)。 原生廣告是以元件為基礎的廣告格式，其中每一項廣告創意 (例如標題、影像、描述和喚起行動文字) 都會當做個別元素傳送到您的應用程式。 原生廣告則目前僅供加入試驗計劃的開發人員使用，但我們很快就要將這項功能提供給所有的開發人員。
定價和可用性 |  新的價格與可用性選項可讓您[排程價格變更](../publish/set-and-schedule-app-pricing.md)和[設定精確的發行日期](../publish/configure-precise-release-scheduling.md)。
Store 分析 API | [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md) 現在提供一個方法，您可用來[下載 CAB 檔案以取得 App 中的錯誤](../monetize/download-the-cab-file-for-an-error-in-your-app.md)。
Store 清單 | Store 清單已藉由新功能而獲得增強，用以吸引潛在使用者： </br>* 您應用程式的 Store 清單現在可以包含[預告片](../publish/app-screenshots-and-images.md#trailers)。 </br></br>* 您可以[匯入和匯出 Store 清單](../publish/import-and-export-store-listings.md)來加快更新，尤其是在您的清單有許多語言版本時。
提交 API | [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 現在可讓您使用 App 提交來包含[預告片](../monetize/manage-app-submissions.md#trailer-object)和[遊戲選項](../monetize/manage-app-submissions.md#gaming-options-object)。
針對性優惠 | [針對性優惠](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)讓您以特定客戶區隔為目標，透過有吸引力的個人化內容提高吸引力、留住客戶並創造營收。

## <a name="samples"></a>範例

### <a name="lunch-scheduler"></a>午餐排程器

[午餐排程器](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)範例可排程您與朋友和同事共用午餐。 您建立午餐約會、邀請朋友前往感興趣的餐廳，接下來應用程式會負責所有受邀對象的午餐管理。 此應用程式會醒目提示下列項目：

- 示範與各種服務整合，例如 Facebook、Microsoft Graph (用於驗證)、圖形作業和好友探索。
- 可搭配 Yelp 和 Bing 地圖來提供餐廳建議。
- 在 UWP app 中納入 Fluent Design 系統元素，包括壓克力風格、顯色和連接動畫。

### <a name="quiz-game"></a>測驗遊戲

[測驗遊戲應用程式 (遠端系統工作階段 API)](https://github.com/microsoft/Windows-appsample-remote-system-sessions) 範例示範如何在測驗遊戲場景的內容中使用遠端系統工作階段 API。 主機傳送問題給近端裝置，然後參與者在自己的裝置上回答問題。

遠端系統工作階段 API 允許裝置裝載其他附近裝置可搜尋的工作階段。 隨後這些裝置可以加入此工作階段，然後將訊息傳送給主機和其他參與者。 
