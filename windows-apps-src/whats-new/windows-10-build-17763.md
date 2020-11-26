---
title: Windows 10 (組建 17763) 的新功能
description: Windows 10 組建 17763 與新的開發人員工具提供由通用 Windows 平台所提供的工具、功能及體驗。
keywords: Windows 10, 17763, 1809
ms.date: 10/03/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 416e3947678de0ba70687d9070c245fdfb4376df
ms.sourcegitcommit: 67c4d4ecda4ffe5f1a233de5e8555ca2228e8489
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2020
ms.locfileid: "94933183"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>適用於開發人員的 Windows 10 (組建 17763) 的新功能

Windows 10 組建 17763 (也稱為 2018 年 10 月更新或版本 1809) 搭配 Visual Studio 2019 與更新的 SDK，提供工具、功能以及體驗來造就不凡的通用 Windows 平台應用程式。 在 Windows 10 上[安裝工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank) 之後，就表示您已經準備好[建立新的通用 Windows 應用程式](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的應用程式程式碼](../porting/index.md)。

這是此版本中 Windows 開發人員會感興趣的新功能和改良功能以及指導方針的集合。 如需新增到 Windows SDK 之新命名空間的完整清單，請參閱 [Windows 10 組建 17763 API 變更](windows-10-build-17763-api-diff.md)。 如需 Windows 10 重點功能的詳細資訊，請參閱 [Windows 10 中有哪些酷功能](https://developer.microsoft.com/windows/windows-10-for-developers)。 此外，請參閱 [Windows 開發人員平台功能](https://developer.microsoft.com/windows/platform/features)以取得過去與未來加入 Windows 平台功能的高階概觀。

## <a name="design--ui"></a>設計與 UI

功能 | 說明
 :------ | :------
應用程式圖示及標誌 | [應用程式圖示和標誌頁面](../design/style/app-icons-and-logos.md)已重寫，現在會顯示最新的 Visual Studio 圖示工具，並提供有關將影像加入至 Microsoft Store 中的應用程式清單的資訊。
設計登陸頁面 | [更新的設計登陸頁面](https://developer.microsoft.com/windows/apps/design)概述了 UWP 設計區域以及有關 Fluent Design 最新新增項目的資訊。
Fluent Design 控制項 | 下列新的 UI 控制項已加入，以增強 Fluent Design System 和應用程式的外觀： </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md) 可讓您在 UI 畫布上的項目內容中顯示一般使用者工作。 </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、[SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) 和 [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) 提供了具有專門功能的按鈕控制項，以增強您的應用程式使用者介面。 </br> * [MenuBar](../design/controls-and-patterns/menus.md) 會在水平列中顯示一組最上層的多個功能表。 </br> * [NavigationView](../design/controls-and-patterns/navigationview.md) 現在支援最上層導覽，適用於您的應用程式導覽選項較少且需要更多內容空間的情況。 </br> * [TreeView](../design/controls-and-patterns/tree-view.md) 已增強，可支援資料繫結、項目範本與拖放功能。
Fluent Design 更新 | 以下 Fluent Design 頁面已進行視覺效果更新和次要變更： </br> * [對齊方式、邊框間距、邊界](../design/layout/alignment-margin-padding.md) </br> * [色彩](../design/style/color.md) </br> * [適用於 Windows 應用程式的 Fluent Design](/windows/apps/fluent-design-system) </br> * [應用程式設計簡介](../design/basics/design-and-ui-intro.md) </br> * [導覽基本知識](../design/basics/navigation-basics.md) </br> * [回應式設計技術](../design/layout/responsive-design.md) </br> * [螢幕大小與中斷點](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [樣式概觀](../design/style/index.md) </br> * [撰寫樣式](../design/style/writing-style.md) </br> 此外，我們已使用全新資訊重寫了以下頁面的內容區域： </br> * [圖示](../design/style/icons.md)現在提供了使用圖示並使其可點按的實用建議。 </br> * [印刷樣式](../design/style/typography.md)整合來自類似文章的資訊，將所有內容放在同一個地方，並包含更新的指引與圖例。
注視輸入與互動 | [注視互動](../design/input/gaze-interactions.md)允許您的應用程式追蹤使用者的注視、注意力，以及根據他們眼球的位置與移動存在。 這項功能可用來作為輔助技術，並在無法使用傳統輸入裝置的情況下，提供遊戲和其他互動的機會。
手寫檢視 | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md) 是 TextBox 和 RichEditBox 的新筆跡輸入介面。 使用者可以使用手寫筆點選文字控制項，將控制項擴展到書寫表面。 本指引說明如何管理及自訂應用程式中的 HandwritingView。
Fluent Design 中的動作 | Fluent Design System 中運動的使用正在不斷發展，建立在計時、加/減速、方向性和重力基本概念之上。 套用這些基本概念將有助於引導使用者使用您的應用程式，並通過反映自然世界將他們其與數位體驗連結。 深入了解下列文章： </br> * [動作概觀](../design/motion/index.md)已更新，以反映這些基本概念。 </br> * [動作實踐](../design/motion/motion-in-practice.md)提供如何在您的應用程式內套用這些基本概念的範例。 它還包含有關「隱含動畫」的資訊，當 XAML 元素的屬性發生變更時，它允許在舊值和新值之間輕鬆插補。 </br> * [方向性和重力](../design/motion/directionality-and-gravity.md)鞏固了使用者的應用程式心智模型。 </br> * [計時和加/減速](../design/motion/timing-and-easing.md)為您應用程式中的動作加入了真實感。 </br> * [XAML屬性動畫](../design/motion/xaml-property-animations.md)允許您直接為 XAML 元素的屬性設定動畫，而無需與基礎組合視覺效果互動。
頁面轉換 | [頁面轉換](../design/motion/page-transitions.md)可在應用程式中的頁面之間導覽使用者。 它們可以幫助使用者了解他們在導覽階層架構中的位置，並提供有關頁面之間關聯性的意見反應。
文字大小調整 | 新的[文字大小調整指引](../design/input/text-scaling.md)說明如何更新您的應用程式，以配合新的文字大小調整行為，這些行為讓使用者能夠跨 OS 和個別的應用程式變更相對字型大小。 相較於使用 [放大鏡] 應用程式 (通常只是放大螢幕區域內的所有內容，並引進自己的可用性問題)、變更顯示器解析度，或依賴 DPI 縮放比例 (根據顯示器和一般觀看距離調整所有內容)，使用者可以快速存取設定以僅調整文字大小，範圍從 100% (預設大小) 到 225%。
工具組 | [Adobe XD 和 Adobe Illustrator 工具組](../design/downloads/index.md)已更新了新功能。 這些設計工具組提供控制項與版面配置範本，可用於設計 UWP 應用程式。
UI 命令 | [UWP 命令基礎結構](../design/basics/commanding-basics.md)的更新包括更好的命令物件封裝 (行為、標籤、圖示、鍵盤快速鍵、便捷鍵和描述) 以及一組標準的常用命令，包括剪下、複製、貼上、退出等等，這就不需要手動設定這些屬性。 </br> 新的 [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) 類別提供了一個基底類別，用於定義在叫用時執行動作之互動式 UI 元素的命令行為。 這是 [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) 的父類別，它公開了一組具有預先定義屬性的標準平台命令。 
Windows UI 程式庫 | [Windows UI 程式庫](/uwp/toolkits/winui/)是一組 NuGet 套件，可提供 UWP 應用程式的控制項和其他使用者介面元素。 這些套件也提供對舊版 Windows 10 的向下相容性，因此即便您的使用者沒有最新版本的作業系統，您的應用程式仍可運作。 </br> 如需 Windows UI 程式庫中之內容的詳細資訊，請參閱 [NuGet 套件中所包含的此 API 命名空間清單](/windows/winui/api/)。

## <a name="develop-windows-apps"></a>開發 Windows 應用程式

功能 | 說明
 :------ | :------
條碼掃描器 | [條碼掃描器](../devices-sensors/pos-barcodescanner.md)文件已重新組織，並使用更多詳細資訊和程式碼片段進行改善。 我們還新增了一個新主題[取得並了解條碼資料](../devices-sensors/pos-barcodescanner-scan-data.md)，其中介紹了如何從條碼掃描器取得和處理資料。
C++/WinRT | [C++/WinRT](../cpp-and-winrt-apis/index.md) 包含此版本的許多新功能、變更和修正。 有一些新的函式和基底類別可以幫助您實作您自己的[集合屬性和集合型別](../cpp-and-winrt-apis/collections.md)；現在，您可以將 [{Binding}](../xaml-platform/binding-markup-extension.md) XAML 標記延伸與 C++/WinRT 執行階段類別一起使用 (如需程式碼範例，請參閱[資料繫結概觀](../data-binding/data-binding-quickstart.md))。 如需此版本中所有新增內容和變更內容的完整說明，請參閱 [C++/WinRT 的新功能](../cpp-and-winrt-apis/news.md)。</br></br>其他新的 C++/WinRT 內容包括：[XAML 自訂控制項](../cpp-and-winrt-apis/xaml-cust-ctrl.md)；[撰寫 COM 元件](../cpp-and-winrt-apis/author-coclasses.md)；[值分類](../cpp-and-winrt-apis/cpp-value-categories.md)；和[強式和弱式參考](../cpp-and-winrt-apis/weak-references.md)。
C++/WinRT 程式碼範例 | 我們在文件中針對主題新增了 250 個 C++/WinRT 程式碼清單，並隨附現有的 C++/CX 程式碼範例。
參與指引 | 我們已更新 [UWP 文件的參與指引](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md)。 這份新指引闡明了對我們文件的外部參與的工作流程和期望。
DirectX 圖形架構 (DXGI) | 已針對遺漏的 DXGI API 新增了新文件，我們在 Windows 10 上提供了一篇關於最佳作法的文章。 </br> * [為了達到最佳效能，請使用 DXGI 翻轉模型](/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model)：提供有關如何在現代化版本的 Windows 上，最大化簡報堆疊中效能和效率的指引。 </br> * [IDXGIOutput6::CheckHardwareCompositionSupport 方法](/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport)：通知應用程式支援硬體伸展。 </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS 列舉](/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags)：描述支援的硬體組合層級。
開始使用 | 我們的[入門](../get-started/index.md)內容已經透過新主題重寫，提供有關剛接觸 Windows 10 的開發人員如何完成以下一般工作的資訊和指引： </br> * [建構表單](../get-started/construct-form-learning-track.md) </br> * [在清單中顯示客戶](../get-started/display-customers-in-list-learning-track.md) </br> * [儲存和載入設定](../get-started/settings-learning-track.md) </br> * [使用檔案](../get-started/fileio-learning-track.md)
地圖樣式表編輯器 | 使用新的[地圖樣式表編輯器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab)應用程式，以互動方式自訂新增至應用程式之地圖的外觀。
Microsoft Learn | 全新的 [Microsoft Learn 網站](https://www.microsoft.com/learning/default.aspx)為 Microsoft 開發人員提供新的實際操作學習和訓練機會。 目前，Microsoft Learn 為 Microsoft 365、Microsoft Azure 和 Windows Server 提供培訓及認證。
記事本 | [[記事本] 已更新](https://blogs.windows.com/windowsexperience/2018/07/11/announcing-windows-10-insider-preview-build-17713/) \(英文\)，新增了縮放、循環尋找/取代，以及 Unix/Linux (LF) 和 Mac (CR) 行尾結束符號的支援。
Project Rome | [Project Rome](/windows/project-rome/) 現在可以在支援的平台和 SDK 之間提供一致的程式設計體驗。 </br>  新的 [Microsoft Graph 通知](/graph/notifications-concept-overview)使用 Project Rome 為您的應用程式提供以人為中心的跨平台通知平台。
螢幕剪取 | 新的 [URI 配置](../launch-resume/launch-screen-snipping.md)允許您的應用程式以程式設計方式開啟新的剪取，或針對註釋啟動帶有特定影像的剪取與繪圖應用程式。
傳統型應用程式中的 UWP 控制項 | Windows 10 現在可讓您在 WPF、Windows Forms 和 C++ Win32 傳統型應用程式中使用 UWP 控制項。 這表示您可以使用只能透過 UWP 控制項 (例如 Windows Ink) 和支援 Fluent Design 系統的控制項存取的最新 Windows 10 UI 功能，來增強現有傳統型應用程式的外觀與風格和功能。 這項功能稱為 *XAML Islands*。 </br> 我們提供了幾種在應用程式中使用 XAML Islands 的方法，具體取決於您使用的應用程式平台。 WPF 和 Windows Forms 應用程式可以使用 [Windows 社群工具組](/windows/uwpcommunitytoolkit/)中的一組控制項，這些控制項提供設計導向的開發經驗。 C++ Win32 應用程式必須使用 [Windows.UI.Xaml.Hosting](/uwp/api/windows.ui.xaml.hosting) 命名空間中的 *UWP XAML 裝載 API* 。 如需詳細資訊，請參閱[傳統型應用程式中的 UWP 控制項](/windows/apps/desktop/modernize/xaml-islands)。 </br> **注意：** 啟用 XAML Islands 的 API 和控制項目前可作為開發人員預覽提供使用。 雖然我們鼓勵您現在在自己的原型程式碼中試用它們，但我們不建議您此時在實際程式碼中使用它們。
Windows Machine Learning | [Windows Machine Learning](/windows/ai/) 已正式推出，可為尖端機器學習模型提供更快速的評估和支援。 為了支援想要將其整合到應用程式中的開發人員，我們建立了一個新的文件網站，其中包含幾個全新和更新的資源： </br> * [教學課程：建立 Windows Machine Learning 傳統型應用程式 (C++)](/windows/ai/get-started-desktop)：本教學課程會示範如何建置簡單的 Windows ML 傳統型應用程式。 </br> * [教學課程：建立 Windows Machine Learning UWP 應用程式 (C#)](/windows/ai/get-started-uwp)：在此逐步教學課程中使用 Windows ML 建立您的第一個 UWP 應用程式。 </br> * [Windows.AI.MachineLearning 命名空間](/uwp/api/windows.ai.machinelearning)：API 參考已針對最新版本的 Windows 10 SDK 進行更新，開發人員現在可以將此 API 用於 Win32 與 UWP 應用程式。
Windows Mixed Reality | 如果顯示器硬體支援，開發人員現在可以要求硬體保護的後端緩衝區紋理，允許應用程式使用來自 PlayReady 等來源的硬體保護內容。 透過使用 [Windows.Graphics.Holographic.HolographicCamera](/uwp/api/windows.graphics.holographic.holographiccamera) 的新屬性以及透過 [Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters) 的 Quad 圖層，可以為主要圖層提供硬體保護支援及設定。

## <a name="iot-core"></a>IoT 核心版

功能 | 說明
 :------ | :------
AssignedAccessSettings | [AssignedAccessSettings 類別](/uwp/api/windows.system.userprofile.assignedaccesssettings)允許呼叫不同的方法和屬性，來存取使用者為特定裝置指派的存取設定。
預設應用程式概觀 | [Windows 10 IoT 核心版預設應用程式](/windows/iot-core/develop-your-app/iotcoredefaultapp)已使用新功能進行更新，例如天氣、筆跡及音訊。
儀表板 | [Windows 10 IoT 核心版儀表板](/windows/iot-core/tutorials/quickstarter/devicesetup)現在允許使用 Dragonboard 410C 或 NXP 的開發人員，將自訂 FFU 刷新至他們的裝置上。
螢幕小鍵盤 | 現在，IoT 裝置的[螢幕小鍵盤](/windows/iot-core/develop-your-app/onscreenkeyboard)使用與 Windows 桌面版相同的觸控式鍵盤元件。 這樣可以實現聽寫模式、IME 支援和一組完整的輸入範圍等功能。
登入對話方塊的標題列 | Windows 10 IoT 核心版現在提供了[為系統對話方塊設定標題列](/windows/iot-core/develop-your-app/signindialogtitlebars) \(部分機器翻譯\) 的選項。
觸控喚醒 | [觸控喚醒](/windows/iot-core/learn-about-hardware/wakeontouch)可讓您的裝置螢幕在不使用時關閉，同時在使用者碰觸螢幕時快速開啟。
Windows.System.Update | 新的 [Windows.System.Update 命名空間](/uwp/api/windows.system.update)支援對系統更新進行互動式控制。 此命名空間僅適用於 Windows 10 IoT 核心版。

## <a name="web-development"></a>Web 開發

功能 | 說明
 :------ | :------
EdgeHTML 18 | Windows 10 2018 年 10 月更新隨附 [EdgeHTML 18](/microsoft-edge/dev-guide)，這是 Microsoft Edge 瀏覽器的最新更新和適用於 UWP 應用程式的 JavaScript 引擎。 EdgeHTML 18 為 Web 驗證 API、新的 WebView 控制項功能和更多功能帶來了現代化和擴充支援！ 在工具方面，EdgeHTML 18 帶入新的 WebDriver 功能和自動更新，以及 Edge DevTools 和 Edge DevTools 通訊協定的增強功能。 如需所有詳細資料，請參閱 [EdgeHTML 18 中的新功能](/microsoft-edge/dev-guide)和[最新 Windows 10 更新 (EdgeHTML 18) 中的 DevTools](/microsoft-edge/devtools-guide/whats-new)。
漸進式 Web 應用程式 | Windows 10 JavaScript 應用程式 (在 *WWAHost.exe* 程序中執行的 Web 應用程式) 現在支援選用的 [每個應用程式背景指令碼](/microsoft-edge/dev-guide#progressive-web-apps)，該指令碼會在任何檢視啟動之前啟動，並在程序中執行一段時間。 以此方式，您可以監視和修改導覽、追蹤導覽狀態、監視導覽錯誤，並在啟動檢視之前執行程式碼。 當在[應用程式資訊清單](/uwp/schemas/appxpackage/appx-package-manifest)中指定為 [`StartPage`](/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application) 時，每個應用程式的檢視 (視窗) 都會作為新 [`WebUIView`](/uwp/api/windows.ui.webui.webuiview) 類別的執行個體公開給指令碼，提供與一般 (Win32) [WebView](/uwp/api/windows.web.ui.iwebviewcontrol) 相同的事件、屬性和方法。
Web API 擴充功能 | Mozilla Developer Network 文件中新增了[舊版 Microsoft API 擴充功能](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)的清單，以用於跨瀏覽器 Web 開發。 這些 API 擴充功能是 Internet Explorer 或 Microsoft Edge 獨有的，並補充了有關 MDN Web 文件中的相容性和和瀏覽器支援的現有資訊。舊版 Microsoft [CSS 延伸模組](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)和 [JavaScript 延伸模組](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也可以使用，您可以在 [Visual Studio Code](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn) 中直接找到 MDN 中的豐富 Web API 資訊。
WebVR | 我們對 [WebVR 開發人員指南](/microsoft-edge/webvr/)做了重大更新，包括完整重新設計的首頁和重新組織的目錄。 我們還編寫了幾個新主題，包括： </br> * [什麼是 WebVR？](/microsoft-edge/webvr/what-is-webvr) 說明何謂 WebVR、為什麼您應該使用它，以及如何開始針對它進行開發。 </br> * [漸進式 Web 應用程式中的 WebVR](/microsoft-edge/webvr/webvr-in-pwas)：了解如何將 WebVR 新增至漸進式 Web 應用程式 (PWA)。 </br> * [WebView 中的 WebVR](/microsoft-edge/webvr/webvr-in-webview)：了解如何將 WebVR 新增至 Windows 10 應用程式中的 WebView 控制項。 </br> * [WebVR 示範](/microsoft-edge/webvr/demos)：請參閱一些使用 Microsoft Edge 和 Windows Mixed Reality 沉浸式頭戴裝置的 WebVR 示範。

## <a name="publish--monetize-windows-apps"></a>發佈 Windows 應用程式以及從中獲利

功能 | 說明
 :------ | :------
MSIX | [MSIX](/windows/msix/overview) 是新的 Windows 應用程式封裝格式，為所有 Windows 應用程式提供現代化的封裝體驗。 開放原始碼的 MSIX 格式保留現有的封裝功能，同時提供現代化的部署功能。
MSIX 封裝工具 | 新的 [MSIX 封裝工具](/windows/msix/mpt-overview)) 允許您以 MSIX 格式重新封裝您現有的傳統型應用程式，即使您無法存取其原始程式碼。 它可以在命令列中執行，也可以透過其互動式 UI 執行。
適用於 MSIX 的 Desktop App Converter 支援 | 您可以使用 [Desktop App Converter](/windows/msix/desktop/source-code-overview)，藉由使用 `-MakeMSIX` 參數輸出 MSIX 套件。
適用於 MSIX 的 MakeAppx.exe 工具支援 | 您可以使用 MakeAppx.exe 工具為 UWP 應用程式或傳統型應用程式建立 MSIX 套件。 此工具包含在 Windows 10 SDK，而且可以從命令提示字元或指令碼檔案使用。 </br> 針對 UWP 應用程式，請參閱[使用 MakeAppx.exe 工具建立應用程式套件](/windows/msix/package/create-app-package-with-makeappx-tool)。 </br> 針對傳統型應用程式，請參閱[手動封裝傳統型應用程式](/windows/msix/desktop/desktop-to-uwp-manual-conversion)。
套件支援架構 | [套件支援架構](/windows/msix/package-support-framework-overview)是開放原始碼套件，可在您無法存取原始碼時，協助將修正程式套用到現有的傳統型應用程式，使其可以在 MSIX 容器中執行。
Store 分析 API | [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md) 現在包含下列新方法： </br> * [取得 UWP 應用程式的深入解析資料](../monetize/get-insights-data-for-your-app.md) </br> * [取得傳統型應用程式的深入解析資料](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [取得傳統型應用程式的升級區塊](../monetize/get-desktop-block-data.md) </br> * [取得傳統型應用程式的升級區塊詳細資料](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>視訊

下列影片自 Fall Creators Update 發行後即已發佈，重點說明 Windows 10 中適用於開發人員的新功能及改良功能。

### <a name="cwinrt"></a>C++/WinRT

C++/WinRT 是撰寫與使用 Windows 執行階段 API 的新方法。 它僅在標頭檔案中實作，以及設計用來提供您現代化應用程式功能的第一級存取。 [觀看影片](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)以了解其運作方式，然後[閱讀開發人員文件](../cpp-and-winrt-apis/index.md)以取得詳細資訊。

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>開發人員入門：在 Window 10 上建立和自訂表單

我們針對 Windows 開發人員的[入門文件](../get-started/index.md)現在提供了基本應用程式開發工作的實際操作經驗。 這段影片將向您逐步說明其中一個主題，並說明如何在您的應用程式中建立表單 UI 的基本概念。 [觀看影片](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)以查看實際操作的程式碼，然後[自行查看主題](../get-started/construct-form-learning-track.md) \(部分機器翻譯\)。

### <a name="enhance-your-bot-with-project-personality-chat"></a>使用個人化聊天專案增強您的機器人

個人化聊天專案允許您為聊天機器人新增可自訂的角色。 藉由與 Microsoft Bot Framework SDK 整合，您可以新增閒聊功能，以更交談式的方式與客戶互動。 [觀看影片](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)以了解如何實作它，然後[試用互動式示範](https://www.microsoft.com/research/project/personality-chat/)以取得實際操作體驗。

### <a name="multi-instance-uwp-apps"></a>多執行個體 UWP 應用程式

Windows 現在允許您執行 UWP 應用程式的多個執行個體，而每個執行個體在其自己的個別處理序中執行。 [觀看影片](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)以了解如何建立支援這項功能的新應用程式，然後[閱讀開發人員文件](../launch-resume/multi-instance-uwp.md)，以取得如何及為何使用這項功能的詳細指引。

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 外掛程式

Unity 的 Xbox Live 外掛程式包含對您的標題加入 Xbox Live 簽章、統計資料、朋友清單、雲端儲存空間和排行榜。 [觀看影片](https://youtu.be/fVQZ-YgwNpY)以深入了解，然後[下載 GitHub 套件](/gaming/xbox-live/get-started/setup-ide/creators/unity-win10/live-cr-unity-win10-nav?WT.mc_id=windowsdocs-twi)即可開始著手。

### <a name="one-dev-question"></a>One Dev Question

在 One Dev Question 影片系列中，資深的 Microsoft 開發人員會談論一系列關於 Windows 開發、團隊文化和歷史的問題。

* [Raymond Chen：關於 Windows 開發與歷史](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman：關於 Windows 開發與歷史](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson：漸進式 Web 應用程式](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann：webhint 工具](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>範例

### <a name="customer-orders-database"></a>客戶訂單資料庫

[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)已更新為使用新的控制項，例如 [DataGrid](/windows/communitytoolkit/controls/datagrid)、[NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) 和 [Expander](/windows/communitytoolkit/controls/expander)。

### <a name="customer-database-tutorial"></a>客戶資料庫教學課程

本[客戶資料庫教學課程](../enterprise/customer-database-tutorial.md)會建立一個基本 UWP 應用程式，以供管理客戶清單，以及介紹企業應用程式開發的實用概念和做法。 它會逐步引導您實作 UI 元素並新增本機 SQLite 資料庫的作業，以及提供連線到遠端 REST 資料庫的簡易指引 (如果您想要進一步了解)。

### <a name="photo-editor-cwinrt"></a>Photo Editor C++/WinRT

[Photo Editor 範例應用程式](https://github.com/Microsoft/Windows-appsample-photo-editor)展示使用 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 語言投影進行開發。 該應用程式可讓您從 **圖片** 庫擷取相片，然後以關聯的相片效果編輯所選的影像。

### <a name="windows-machine-learning"></a>Windows Machine Learning

[Windows-Machine-Learning](https://github.com/Microsoft/Windows-Machine-Learning) 存放庫已更新，可與最新的 Windows 10 SDK 一起使用，並包含使用 C#、C++ 和 JavaScript 撰寫的範例。

### <a name="xaml-hosting-api"></a>XAML 裝載 API

[XAML 裝載 API 範例](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)是一個 Win32 傳統型應用程式，它使用 UWP XAML 裝載 API (也稱為 XAML Islands) 醒目提示各種案例。 該專案在資源庫樣式的簡報中整合了 Windows Ink、Media Player 和導覽檢視控制項。 除了一般控制項使用方式之外，該範例還示範了處理 XAML 和原生 Windows 事件/訊息，以及基本 XAML 資料繫結。