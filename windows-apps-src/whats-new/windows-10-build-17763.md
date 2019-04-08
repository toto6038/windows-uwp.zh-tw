---
title: Windows 10 中適合開發人員的新功能、工具與特色
description: Windows 10 組建 17763 和全新開發人員工具提供工具、 功能及由通用 Windows 平台提供的體驗。
keywords: 最新消息，功能新的更新，更新功能，新的 Windows 10 最新開發人員，17763
ms.date: 10/03/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2b172844e75d9af3d0112e03f155708af3ca6bed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637743"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>What's New in Windows 10 開發人員建置 17763 時

Windows 10 組建 17763 (也稱為年 10 月 2018年更新或版本 1809年)，搭配 Visual Studio 2017 和更新的 SDK，可提供工具、 功能和體驗，以建立引人注目的通用 Windows 平台應用程式。 在 Windows 10 上[安裝工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows app](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

這是此版本中 Windows 開發人員會感興趣的新功能和改良功能以及指導方針的集合。 新的命名空間新增至 Windows SDK 的完整清單，請參閱 < [Windows 10 組建 17763 API 變更](windows-10-build-17763-api-diff.md)。 如需 Windows 10 重點功能的詳細資訊，請參閱 [Windows 10 中有哪些酷功能](https://go.microsoft.com/fwlink/?LinkId=823181)。 此外，請參閱 [Windows 開發人員平台功能](https://developer.microsoft.com/windows/platform/features)以取得過去與未來加入 Windows 平台功能的高階概觀。

## <a name="design--ui"></a>設計與 UI

功能 | 描述
 :------ | :------
應用程式圖示及標誌 | [應用程式圖示和標誌頁面](../design/style/app-icons-and-logos.md)已重寫，現在會顯示最新的 Visual Studio 圖示工具，並將影像加入至您的應用程式清單中的 Microsoft Store 提供的資訊。
設計登陸頁面 | [更新登陸頁面的設計](https://developer.microsoft.com/windows/apps/design)已在摘要概觀 UWP 設計領域和 Fluent 設計的最新新增項目上的資訊。
Fluent 設計控制項 | 下列新的 UI 控制項已經加入，以增強 Fluent Design System 與您的應用程式的 apparence: </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md)可讓您的 UI 畫布上的項目內容中顯示一般使用者工作。 </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)， [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button)，以及[ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)提供特製化的功能，來加強您的應用程式使用者介面中的按鈕控制項。 </br> * [功能表列](../design/controls-and-patterns/menus.md)水平資料列顯示一組多個最上層的功能表。 </br> * [NavigationView](../design/controls-and-patterns/navigationview.md)現在支援最上層導覽中，您的應用程式具有較少的導覽選項和內容需要更多空間的情況。 </br> * [TreeView](../design/controls-and-patterns/tree-view.md)已增強來支援資料繫結項目範本，並拖曳和卸除。
Fluent Design 更新 | 視覺效果更新和次要的變更進行了下列 Fluent 設計頁面： </br> * [對齊方式，與邊框距離、 邊界](../design/layout/alignment-margin-padding.md) </br> * [色彩](../design/style/color.md) </br> * [Windows 應用程式的 Fluent 設計](../design/fluent-design-system/index.md) </br> * [應用程式設計的簡介](../design/basics/design-and-ui-intro.md) </br> * [瀏覽的基本概念](../design/basics/navigation-basics.md) </br> * [回應式設計的技術](../design/layout/responsive-design.md) </br> * [螢幕大小和中斷點](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [樣式概觀](../design/style/index.md) </br> * [撰寫樣式](../design/style/writing-style.md) </br> 此外，我們已重寫其內容區域的全新有關的下列頁面： </br> * [圖示](../design/style/icons.md)現在提供使用圖示，並且讓它們可點按的實用建議。 </br> * [印刷樣式](../design/style/typography.md)將類似的文章，將所有內容放在同一個地方更新的指引與圖例資訊合併。
視線輸入和互動 | [視線互動](../design/input/gaze-interactions.md)允許您的應用程式，以追蹤使用者的視線、 注意及根據位置和移動的眼睛的目前狀態。 這項功能可用來當做輔助技術，並提供機會讓遊戲和傳統的輸入的裝置沒有可用的其他互動式案例。
手寫檢視 | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md)文字方塊和 RichEditBox 是在新的筆跡輸入的介面。 使用者可以點選文字控制項，其畫筆書寫表面至展開的控制項。 本指南說明如何管理及自訂應用程式中的 HandwritingView。
Fluent 設計的影片 | Fluent Design System 中的動作使用的演變，基礎的執行時間、 簡化、 方向性和重力基本概念。 套用這些基本概念可協助引導您的應用程式，使用者並讓他們與他們的數位體驗，透過反映自然的世界。 進一步了解下列文章： </br> * [影片概觀](../design/motion/index.md)已更新以反映這些基本概念。 </br> * [影片中練習](../design/motion/motion-in-practice.md)提供有關如何套用這些應用程式內的基本概念的範例。 它也包含隱含的動畫，允許 XAML 元素的屬性變更時的舊和新值之間的簡單插補的詳細資訊。 </br> * [方向和重力](../design/motion/directionality-and-gravity.md)solidifies 使用者的心智模型，您的應用程式。 </br> * [計時，以及減輕](../design/motion/timing-and-easing.md)加入您的應用程式中的動畫的真實性。 </br> * [XAML 屬性動畫](../design/motion/xaml-property-animations.md)可讓您直接以動畫顯示屬性的 XAML 項目，而不需要與基礎組合視覺效果互動。
頁面轉換 | [頁面轉換](../design/motion/page-transitions.md)瀏覽應用程式中的頁面之間的使用者。 它們幫助使用者了解它們的導覽階層架構中的位置，並提供意見反應頁面之間的關聯性。
文字大小調整 | 新[調整指引的文字](../design/input/text-scaling.md)說明如何更新您的應用程式，以配合調整行為，讓使用者變更相對字型大小跨 OS 和個別的應用程式的新文字。 而不是使用 [放大鏡] 應用程式 （這通常只是可以放大螢幕上的區域內的所有項目，並引進自己的可用性問題）、 變更顯示器解析度，或依賴 DPI 縮放比例 （這會顯示和一般檢視為基礎的所有項目調整大小距離），使用者可以快速存取設定，以可調整大小只是文字，範圍從 100%（預設大小） 225%。
工具組 | [Adobe XD 和 Adobe Illustrator 的工具組](../design/downloads/index.md)已更新為新的功能。 這些設計工具組提供設計 UWP 應用程式的控制項和版面配置範本。
UI 命令 | 若要更新[UWP 命令基礎結構](../design/basics/commanding-basics.md)包括更好的封裝，將命令物件 （行為、 標籤、 圖示、 鍵盤快速鍵、 便捷鍵和描述） 和一組標準的常用命令，包括剪下、 複製貼上、 結束、 等等，這就不需要手動設定這些屬性。 </br> 新[XamlUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.xamluicommand)類別提供基底類別來定義命令的行為執行動作時叫用互動式 UI 項目。 這是父類別[StandardUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.standarduicommand)，而這會公開一組標準的平台命令，使用預先定義的屬性。 
Windows UI 程式庫 | [Windows 的 UI 程式庫](https://aka.ms/winui-docs)是一組控制項與其他使用者介面項目為 UWP 應用程式提供的 NuGet 套件。 這些套件也會與舊版的 Windows 10 相容，因此即使您的使用者沒有最新的作業系統版本，適用於您的應用程式。 </br> 如需有關功能的 Windows UI 程式庫的詳細資訊，請參閱[這份 API NuGet 套件中包含的命名空間。](https://docs.microsoft.com/uwp/api/overview/winui/)

## <a name="develop-windows-apps"></a>開發 Windows 應用程式

功能 | 描述
 :------ | :------
條碼掃描器 | [條碼掃描器](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner)文件已重新組織，並改善了更多詳細資料和程式碼片段。 我們也新增了新的主題中，[取得，並了解條碼資料](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner-scan-data)，其中說明如何取得及使用條碼掃描器中的資料。
C++/WinRT | [C + + /cli WinRT](https://aka.ms/cppwinrt)包含許多新功能、 變更和此版本中修正。 有新的函式和基底類別，以支援您實作您自己[集合屬性和集合型別](/windows/uwp/cpp-and-winrt-apis/collections); 而您現在可以使用[{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) XAML 標記延伸，以您 C + + /cli WinRT執行階段類別 (如程式碼範例，請參閱 <<c8> [ 資料繫結概觀](/windows/uwp/data-binding/data-binding-quickstart))。 新的和變更在此版本中的所有內容的完整說明，請參閱[的新 C + + /cli WinRT](../cpp-and-winrt-apis/news.md)。</br></br>其他新的 C + + /cli WinRT 內容包括：[XAML 的自訂控制項](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl);[作者 COM 元件](/windows/uwp/cpp-and-winrt-apis/author-coclasses);[值分類](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories); 並[強式和弱式參考](../cpp-and-winrt-apis/weak-references.md)。
C + + /cli WinRT 程式碼範例 | 我們已新增 250 C + + /cli WinRT 程式碼清單主題文件中隨附的現有 C + + /CX 程式碼範例。
參與的指引 | 我們已更新[我們提供的指引](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md)UWP 說明文件。 在工作流程和外部的投稿文章到我們的文件的期望，將釐清這篇新指導。
DirectX 圖形架構 (DXGI) | 有遺漏的 DXGI Api 已新增新的文件，並在 Windows 10 上呈現時，我們還提供有關最佳做法的文章。 </br> * [為了達到最佳效能，使用 DXGI 翻頁動畫模型](https://docs.microsoft.com/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model):提供如何最大化效能和效率上現代化版本的 Windows presentation 堆疊中的指引。 </br> * [IDXGIOutput6::CheckHardwareCompositionSupport 方法](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport):通知應用程式，支援的硬體自動縮放。 </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS 列舉](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags):描述支援的硬體組合的層級。
開始使用 | 我們[開始](../get-started/index.md)內容具有已 revitalized 新主題，提供資訊和指導方針到 Windows 10 中新的開發人員可能如何完成下列的一般工作： </br> * [建構表單](../get-started/construct-form-learning-track.md) </br> * [顯示在清單中的客戶](../get-started/display-customers-in-list-learning-track.md) </br> * [儲存及載入設定](../get-started/settings-learning-track.md) </br> * [使用的檔案](../get-started/fileio-learning-track.md)
地圖樣式表編輯器 | 使用新[地圖樣式表編輯器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab)應用程式以互動方式自訂對應，您將新增至您的應用程式的外觀。
Microsoft 了解 | 新[Microsoft 了解站台](https://www.microsoft.com/learning/default.aspx)為 Microsoft 開發人員提供新的實際操作學習和訓練機會。 目前，Microsoft 了解提供的 Microsoft 365，Microsoft Azure、 Office 365 和 Windows Server 培訓及認證。
記事本 | [已更新 「 記事本 」](https://aka.ms/ant-man)新增、 縮放、 循環尋找/取代，以及支援 Unix/Linux (LF) 和 Mac (CR) 行尾結束符號。
Project Rome | [Project Rome](https://docs.microsoft.com/windows/project-rome/)現在支援的平台和 Sdk 之間提供一致的程式設計經驗。 </br>  新[Microsoft Graph 通知](https://developer.microsoft.com/graph/docs/concepts/notifications-concept-overview)使用 Project Rome 提供您的應用程式的人為中心、 跨平台通知平台。
螢幕 剪取 | 新[URI 配置](../launch-resume/launch-screen-snipping.md)可讓您的應用程式以程式設計方式開啟新的剪取，或啟動特定的映像，註釋剪取 & 草圖應用程式。
桌面應用程式中的 UWP 控制項 | Windows 10 現在可讓您在 WPF、Windows Forms 和 C++ Win32 傳統型應用程式中使用 UWP 控制項。 這表示您可以使用只能透過 UWP 控制項 (例如 Windows Ink) 和支援 Fluent Design 系統的控制項存取的最新 Windows 10 UI 功能，來增強現有傳統型應用程式的外觀與風格和功能。 這項功能稱為*XAML 群島*。 </br> 我們提供數種方式使用 XAML 群島，在您的應用程式，根據您使用的應用程式平台。 WPF 和 Windows Forms 應用程式可以使用一組中的控制項[Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)，提供設計導向的開發經驗。 C + + Win32 應用程式必須使用*UWP XAML 裝載 API*中[Windows.UI.Xaml.Hosting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting)命名空間。 如需詳細資訊，請參閱 <<c0> [ 桌面應用程式中的 UWP 控制項](../xaml-platform/xaml-host-controls.md)。 </br> **注意：** Api 和控制項，可讓 XAML 群島是目前可供開發人員預覽。 雖然我們鼓勵您試用看看在自己的原型程式碼現在，我們不建議，您使用它們在實際程式碼這一次。
Windows Machine Learning | [Windows Machine Learning](https://docs.microsoft.com/windows/ai/)已現在正式推出，提供最先進的機器學習服務模型更快速的評估和支援等功能。 為了支援想要將它整合到他們的應用程式開發人員，我們建立了新的文件網站有幾個全新和更新的資源： </br> * [教學課程：建立 Windows 機器學習桌面應用程式 （c + +）](https://docs.microsoft.com/windows/ai/get-started-desktop):本教學課程會示範如何建置簡單的 Windows ML 應用程式，適用於桌面。 </br> * [教學課程：建立 Windows 機器學習服務 UWP 應用程式 (C#)](https://docs.microsoft.com/windows/ai/get-started-uwp):在此逐步教學課程中，使用 Windows ML 建立第一個 UWP 應用程式。 </br> * [Windows.AI.MachineLearning 命名空間](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning):已更新最新版的 Windows 10 SDK，API 參考和開發人員現在可以使用此 API 的 Win32 與 UWP 應用程式。
Windows Mixed Reality | 開發人員現在可以要求硬體保護四分之一紋理支援顯示器硬體中，如果允許使用硬體保護內容的來源，例如 PlayReady 的應用程式。 硬體保護支援及設定，適用於主要圖層所使用的新屬性[Windows.Graphics.Holographic.HolographicCamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera)，並透過四層[Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters)。

## <a name="iot-core"></a>IoT 核心版

功能 | 描述
 :------ | :------
AssignedAccessSettings | [AssignedAccessSettings 類別](https://docs.microsoft.com/uwp/api/windows.system.userprofile.assignedaccesssettings)可讓呼叫不同的方法和屬性，存取使用者的指派給特定裝置的存取設定。
預設應用程式概觀 | [Windows 10 IoT Core 預設應用程式](https://docs.microsoft.com/windows/iot-core/develop-your-app/iotcoredefaultapp)已更新新功能和功能，例如天氣、 筆跡，及音訊。
儀表板 | [Windows 10 Iot Core 儀表板](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup)現在可讓開發人員使用 Dragonboard 410 C 或 NXP flash 自訂 FFUs 到他們的裝置。
螢幕小鍵盤 | [螢幕小鍵盤的 IoT 裝置](https://docs.microsoft.com/windows/iot-core/develop-your-app/onscreenkeyboard)現在會使用相同的觸控式鍵盤元件，為 Windows 桌面版。 這可讓功能，例如聽寫模式、 輸入法支援，以及一組完整的輸入範圍。
登入對話方塊的標題列 | Windows 10 IoT 核心版現在提供設定選項[標題列的系統對話方塊](https://docs.microsoft.com/windows/iot-core/develop-your-app/signindialogtitlebars)。
線上醒機觸控 | [線上醒機觸控](https://docs.microsoft.com/windows/iot-core/learn-about-hardware/wakeontouch)讓不在使用時，同時快速開啟使用者碰觸其螢幕關閉您的裝置螢幕。
Windows.System.Update | 新[Windows.System.Update 命名空間](https://docs.microsoft.com/uwp/api/windows.system.update)可讓系統更新的互動式控制。 此命名空間僅適用於 Windows 10 IoT 核心版。

## <a name="web-development"></a>Web 開發

功能 | 描述
 :------ | :------
EdgeHTML 18 | Windows 10 年 10 月 2018 update 隨附[EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide)、 最新 Microsoft Edge 瀏覽器和 UWP 應用程式的 JavaScript 引擎的更新。 EdgeHTML 18 帶來現代化和擴充支援 Web 驗證 API、 新的 WebView 控制項功能和更多功能 ！ 在工具方面，EdgeHTML 18 帶入 WebDriver 的新功能及自動更新和增強功能的 Edge DevTools 和 Edge DevTools 通訊協定。 請參閱[EdgeHTML 18 中最新消息](https://docs.microsoft.com/microsoft-edge/dev-guide)並[DevTools 中最新的 Windows 10 更新 (EdgeHTML 18)](https://docs.microsoft.com/microsoft-edge/devtools-guide/whats-new)所有詳細資料。
漸進式 Web 應用程式 | Windows 10 的 JavaScript 應用程式 (web 應用程式中執行*WWAHost.exe*程序) 現在都支援選擇性[每個應用程式背景指令碼](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide#progressive-web-apps)開始之前的任何檢視會啟動且執行持續時間程序。 以此方式，您可以監視和修改導覽、 追蹤所有巡覽的狀態、 監視瀏覽錯誤，並先檢視會啟動執行程式碼。 當指定為[ `StartPage` ](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application)中您[應用程式資訊清單](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest)，每個應用程式的檢視 (windows) 中公開的新執行個體的指令碼[ `WebUIView` ](https://docs.microsoft.com/en-us/uwp/api/windows.ui.webui.webuiview)類別，為一般 (Win32) 提供相同的事件、 屬性和方法[WebView](https://docs.microsoft.com/en-us/uwp/api/windows.web.ui.iwebviewcontrol)。
Web API 擴充功能 | 一份[舊版的 Microsoft API 擴充功能](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)已新增至跨瀏覽器 web 開發的 Mozilla Developer Network 文件。 這些 API 擴充功能是 Internet Explorer 或 Microsoft Edge 中，唯一的並補充 MDN 的 web 文件中的相容性和瀏覽器支援的現有資訊。舊版的 Microsoft [CSS 延伸模組](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)並[JavaScript 延伸模組](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也可以使用，而且您可以找到豐富的 web API 資訊從 MDN 中直接呈現[Visual Studio Code。](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)
WebVR | 我們做了重大更新[WebVR 開發人員指南](https://docs.microsoft.com/microsoft-edge/webvr/)，包括完整重新設計的首頁上和組織的目錄。 我們也有寫入數個新的主題，包括： </br> * [什麼是 WebVR？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr) 說明何謂 WebVR、 為什麼您應該使用它，以及如何開始開發它。 </br> * [漸進式 Web 應用程式中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas):了解如何新增 WebVR 漸進式 Web App (PWA)。 </br> * [WebView 中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview):了解如何在 Windows 10 應用程式的 WebView 控制項中加入 WebVR。 </br> * [WebVR 示範](https://docs.microsoft.com/microsoft-edge/webvr/demos):請參閱使用 Microsoft Edge 和 Windows Mixed Reality 沈浸式耳機某些 WebVR 示範。

## <a name="publish--monetize-windows-apps"></a>發佈 Windows 應用程式以及從中獲利

功能 | 描述
 :------ | :------
MSIX | [MSIX](https://docs.microsoft.com/windows/msix/overview)是提供所有 Windows 應用程式的最新的封裝體驗的新 Windows 應用程式封裝格式。 開放原始碼的 MSIX 格式保留現有的封裝功能，同時提供現代化的部署功能。
MSIX 封裝工具 | 新[MSIX 封裝工具](https://docs.microsoft.com/windows/msix/mpt-overview)) 可讓您重新封裝您現有的傳統型應用程式 MSIX 的格式，即使您沒有存取其原始程式碼。 它可以執行在命令列中，或透過其互動的 UI。
Desktop App Converter MSIX 的支援 | 您可以使用[Desktop App Converter](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)輸出的 MSIX 封裝，使用`-MakeMSIX`參數。
MSIX MakeAppx.exe 工具支援 | 您可以使用 MakeAppx.exe 工具建立 MSIX 封裝 UWP 應用程式或傳統桌面應用程式。 此工具包含在 Windows 10 SDK，而且可以從命令提示字元或指令碼檔案使用。 </br> 針對 UWP 應用程式，請參閱[MakeAppx.exe 工具建立應用程式套件](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。 </br> 針對桌面應用程式，請參閱[手動封裝傳統型應用程式](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)。
封裝支援架構 | [封裝支援架構](https://docs.microsoft.com/windows/msix/package-support-framework-overview)是開放原始碼套件，可協助您套用修正程式到您現有的桌面應用程式時您沒有存取權的原始程式碼，使其可以 MSIX 容器中執行。
儲存分析 API | [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md)現在包含下列新方法： </br> * [取得您的 UWP 應用程式的深入解析資料](../monetize/get-insights-data-for-your-app.md) </br> * [取得您的桌面應用程式的深入解析資料](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [取得您的桌面應用程式升級的區塊](../monetize/get-desktop-block-data.md) </br> * [取得您的桌面應用程式升級封鎖詳細資料](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>影片

下列影片自 Fall Creators Update 發行後即已發佈，重點說明 Windows 10 中適用於開發人員的新功能及改良功能。

### <a name="cwinrt"></a>C++/WinRT

C + + /cli WinRT 是撰寫和使用 Windows 執行階段 Api 的新方式。 它僅在標頭檔中實作，並為您提供現代化的應用程式功能的第一級存取而設計。 [觀看影片](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)若要了解其運作方式，然後[閱讀開發人員文件](../cpp-and-winrt-apis/index.md)如需詳細資訊。

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>開始進行開發：建立和自訂 Windows 10 上的表單

我們[開始使用 docs](../get-started/index.md)的 Windows 開發人員現在提供實際操作經驗與基本應用程式開發工作。 這段影片將逐步引導您透過這些主題，其中，並說明如何在您的應用程式中建立表單 UI 的基本概念。 [觀看影片](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)以查看程式碼的動作，然後[自行查看主題。](https://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>增強您的機器人專案個性交談

專案的特質交談可讓您將可自訂的角色新增至您的聊天機器人。 藉由整合 Microsoft Bot Framework sdk，您可以新增更交談式的方法，來與客戶互動的小型明瞭功能。 [觀看影片](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)以了解如何實作它，然後[試用互動式示範](https://aka.ms/PersonalityChat)實際操作體驗。

### <a name="multi-instance-uwp-apps"></a>多執行個體 UWP app

Windows 現在可讓您使用在自己個別的處理序中執行的 UWP 應用程式的多個執行個體。 [觀看影片](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)以了解如何建立新的應用程式支援這項功能，然後[閱讀開發人員文件](../launch-resume/multi-instance-uwp.md)詳細指引如何和為什麼使用這項功能。

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 外掛程式

Unity 的 Xbox Live 外掛程式包含支援加入您的標題中的 Xbox Live 簽署、 統計資料、 朋友清單、 雲端存放裝置和排行榜。 [觀看影片](https://youtu.be/fVQZ-YgwNpY)若要進一步了解，然後[下載 GitHub 套件](https://aka.ms/UnityPlugin)開始著手。

### <a name="one-dev-question"></a>一個開發人員問題

在一個開發人員問題影片系列中，資深的 Microsoft 開發人員會討論一系列有關 Windows 開發、 team 文化特性和歷程記錄的問題。

* [Raymond Chen 在 Windows 開發和歷程記錄](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman Windows 開發和歷程記錄](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson 漸進式 Web 應用程式](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann webhint 工具](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>範例

### <a name="customer-orders-database"></a>客戶訂單資料庫

[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)已更新為使用新的控制項，如[DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid)， [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)，以及[Expander](https://docs.microsoft.com/windows/communitytoolkit/controls/expander)。

### <a name="customer-database-tutorial"></a>客戶資料庫教學課程

[客戶資料庫教學課程](../enterprise/customer-database-tutorial.md)會建立基本的 UWP 應用程式來管理客戶的清單，並介紹概念和做法適用於企業應用程式開發。 它會逐步引導您實作的 UI 項目，並新增對本機 SQLite 資料庫中，執行的作業，並提供鬆散連接到遠端的其餘部分資料庫，如果您想要更進一步的指引。

### <a name="photo-editor-cwinrt"></a>Photo Editor C + + /cli WinRT

[Photo Editor 範例應用程式](https://github.com/Microsoft/Windows-appsample-photo-editor)展示使用開發[C + + /cli WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)語言投影。 應用程式可讓您擷取相片**圖片**程式庫，然後編輯選定的映像相關聯的相片效果。

### <a name="windows-machine-learning"></a>Windows Machine Learning

[Windows Machine Learning](https://github.com/Microsoft/Windows-Machine-Learning)存放庫已更新為使用最新的 Windows 10 SDK，並包含範例撰寫的C#，c + + 和 JavaScript。

### <a name="xaml-hosting-api"></a>XAML 裝載 API

[XAML 裝載 API 範例](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)會反白顯示使用裝載 API （也稱為 XAML 群島） UWP XAML 的各種的案例的 Win32 桌面應用程式。 專案會包含在資源庫樣式簡報中的 Windows Ink、 Media Player 和導覽檢視控制項。 一般外部控制項使用方式，此範例也會示範處理 XAML 和原生 Windows 事件/訊息和基本 XAML 資料繫結。
