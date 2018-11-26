---
title: Windows 10 中適合開發人員的新功能、工具與特色
description: Windows 10 組建 17763 和新的開發人員工具提供提供工具、 功能以及由通用 Windows 平台的體驗。
keywords: 新功能，新，更新，多項更新，功能，新，Windows 10，最新，開發人員，17763
ms.date: 10/03/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: dc18577015db5384c2a1f13e8a48758634a053a5
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7691689"
---
# <a name="whats-new-in-windows-10-for-developers-build-17763"></a>適用於開發人員，組建 17763 的 Windows 10 中的新功能

Windows 10 組建 17763 (也稱為年 10 月 2018年更新或版本 1809年)，搭配 Visual Studio 2017 與更新的 SDK，提供工具、 功能以及體驗來造就不凡的通用 Windows 平台應用程式。 在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

這是此版本中 Windows 開發人員會感興趣的新功能和改良功能以及指導方針的集合。 新的命名空間新增到 Windows SDK 的完整清單，請參閱[Windows 10 組建 17763 API 變更](windows-10-build-17763-api-diff.md)。 如需 Windows 10 重點功能的詳細資訊，請參閱 [Windows10 中有哪些酷功能](http://go.microsoft.com/fwlink/?LinkId=823181)。 此外，請參閱 [Windows 開發人員平台功能](https://developer.microsoft.com/windows/platform/features)以取得過去與未來加入 Windows 平台功能的高階概觀。

## <a name="design--ui"></a>設計與 UI

功能 | 描述
 :------ | :------
應用程式圖示及標誌 | [應用程式圖示及標誌頁面](../design/style/app-icons-and-logos.md)已重新撰寫，和現在會顯示最新的 Visual Studio 圖示工具，並將影像新增到您的應用程式清單，在 Microsoft Store 中提供的資訊。
設計登陸頁面 | [更新登陸頁面的設計](https://developer.microsoft.com/windows/apps/design)具備 UWP 設計區域和 Fluent 設計最新新增項目上的資訊在快速的概觀。
Fluent 設計控制項 | 下列新的 UI 控制項已新增，以增強 Fluent 設計系統與您的應用程式的 apparence: </br> * [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md)可讓您顯示一般使用者的工作項目在您的 UI 畫布上的內容中。 </br> * [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、 [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button)，以及[ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)提供按鈕控制項具有特殊功能來增強您的應用程式使用者介面。 </br> * [功能表列](../design/controls-and-patterns/menus.md)會顯示水平列中的一組的多個最上層的功能表。 </br> * [NavigationView](../design/controls-and-patterns/navigationview.md)現已支援頂端的瀏覽，對於您的應用程式有數目較少的瀏覽選項且需要更多空間內容的案例。 </br> * [樹狀檢視](../design/controls-and-patterns/tree-view.md)已增強對支援資料繫結，項目範本，以及拖。
Fluent Design 更新 | 下列的 Fluent Design 頁面已經做視覺更新和次要變更： </br> * [對齊方式、 邊框間距，邊界](../design/layout/alignment-margin-padding.md) </br> * [色彩](../design/style/color.md) </br> * [Windows 應用程式的 fluent 設計](../design/fluent-design-system/index.md) </br> * [應用程式設計簡介](../design/basics/design-and-ui-intro.md) </br> * [瀏覽基本知識](../design/basics/navigation-basics.md) </br> * [回應式設計技術](../design/layout/responsive-design.md) </br> * [螢幕大小與中斷點](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) </br> * [樣式概觀](../design/style/index.md) </br> * [撰寫樣式](../design/style/writing-style.md) </br> 此外，我們已經改寫下列頁面以其內容區域上的所有新資訊： </br> * [圖示](../design/style/icons.md)現在會提供實際使用的圖示，並讓他們成為可點選的建議。 </br> * [印刷樣式](../design/style/typography.md)中的資訊從類似的文章，將所有項目放在單一位置的已更新的指導方針與圖例。
注視輸入和互動 | [注視互動](../design/input/gaze-interactions.md)可讓您的應用程式追蹤使用者的注視、 注意力，以及根據他們眼球的移動與位置存在。 這項功能可做的輔助技術，並提供機會用於遊戲以及其他互動式案例，其中傳統輸入的裝置就無法使用。
手寫檢視 | [HandwritingView](../design/controls-and-patterns/text-handwriting-view.md)是 TextBox 和 RichEditBox 的新的筆墨輸入的表面。 使用者可以展開控制項書寫表面其手寫筆點選文字控制項。 本指南說明如何管理和自訂在您的應用程式中 HandwritingView。
在 Fluent 設計的動作 | Fluent 設計系統中的動作使用已發展，建置在計時、 加/減速、 方向以及重力的基本概念。 套用這些基礎可協助引導您的應用程式，使用者和由反映自然界將它們連接與及其數位體驗。 了解這些文章中的多個： </br> * [動作概觀](../design/motion/index.md)已經更新以反映這些基本概念。 </br> * [實務影片](../design/motion/motion-in-practice.md)提供如何套用這些基礎您應用程式內的範例。 它也包含隱含動畫，允許針對 XAML 元素的屬性變更時的舊和新值之間輕鬆內插補點的詳細資訊。 </br> * [方向性和重力](../design/motion/directionality-and-gravity.md)強化使用者的心理模式應用程式。 </br> * [計時和加/減速](../design/motion/timing-and-easing.md)新增擬真度到您的應用程式中的動作。 </br> * [XAML 屬性動畫](../design/motion/xaml-property-animations.md)可讓您直接屬性產生動畫效果的 XAML 元素，而不需要與基礎組合視覺效果進行互動。
頁面轉換 | 使用者在應用程式中的頁面之間瀏覽的[頁面轉換](../design/motion/page-transitions.md)。 它們能協助使用者了解它們的瀏覽階層中的位置，並提供有關在頁面之間關聯性的意見反應。
文字大小調整 | 新的[文字縮放指南](../design/input/text-scaling.md)說明如何更新您的應用程式，以容納的新文字縮放比例行為，這提供使用者變更相對字型大小跨作業系統和個別應用程式的能力。 而不是使用放大鏡 app （這通常只會放大螢幕的區域內的所有項目，並引進了自己的可用性問題）、 變更顯示器解析度，或依賴 DPI 縮放比例 （這會調整大小，根據顯示和一般檢視的所有項目距離），使用者可以快速存取調整大小只是文字，涵蓋範圍可從 100%（預設大小） 的設定 225%。
工具組 | [Adobe XD 和 Adobe Illustrator 工具組](../design/downloads/index.md)已更新為新的功能。 這些設計工具組提供控制項與版面配置範本可用於設計 UWP 應用程式。
UI 命令功能 | [UWP 命令基礎結構](../design/basics/commanding-basics.md)的更新包含更好的命令物件 （行為、 標籤、 圖示、 鍵盤快速鍵、 便捷鍵和描述） 封裝和一組標準的常用的命令，包括剪下、 複製、 貼上、 結束、 等。，這不需要手動設定這些屬性。 </br> 新的[XamlUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.xamluicommand)類別提供基底類別 deveining 命令行為的執行動作時叫用的互動式 UI 元素。 這是[StandardUICommand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.standarduicommand)，這會公開一組預先定義的屬性的標準的平台命令的父類別。 
Windows UI 文件庫 | [Windows UI 文件庫](https://aka.ms/winui-docs)是一組提供控制項與其他使用者介面元素用於 UWP 應用程式的 NuGet 套件。 這些套件也是使用較舊版本的 Windows 10 相容的所以您的應用程式運作方式，即使您的使用者沒有最新的 OS 版本。 </br> 如需什麼是 Windows UI 文件庫中的詳細資訊，請參閱[NuGet 套件中包含的 API 命名空間的這份清單。](https://docs.microsoft.com/uwp/api/overview/winui/)

## <a name="develop-windows-apps"></a>開發 Windows 應用程式

功能 | 描述
 :------ | :------
條碼掃描器 | [條碼掃描器](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner)的文件已重新組織，並改善了更多詳細資料和程式碼片段。 我們也新增了新的主題中，[取得並了解條碼資料](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-barcodescanner-scan-data)，正好說明如何取得並搭配從條碼掃描器的資料。
C++/WinRT | [C + + /winrt](https://aka.ms/cppwinrt)包含許多的新功能、 變更，以及此版本的修正程式。 有新函式和實作您自己的[集合屬性和集合類型](/windows/uwp/cpp-and-winrt-apis/collections); 支援您的基底類別您現在可以使用[{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) XAML 標記延伸，使用您 C + + /winrt 執行階段類別 （如需程式碼範例，請參閱[資料繫結概觀](/windows/uwp/data-binding/data-binding-quickstart)）。 如需完整描述的所有項目全新和已變更，在此版本中，請參閱[新在 C + + /winrt](../cpp-and-winrt-apis/news.md)。</br></br>其他新的 C + + /winrt 項內容包括：[自訂的 XAML 控制項](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl);[撰寫 COM 元件](/windows/uwp/cpp-and-winrt-apis/author-coclasses);[值類別](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories);與[強式和弱式參考](../cpp-and-winrt-apis/weak-references.md)。
C + + /winrt 程式碼範例 | 我們已經新增 250 C + + /winrt 程式碼清單主題中我們的文件，伴隨的現有的 C + + /CX 程式碼範例。
發表指導方針 | 如需我們 UWP 文件，我們已更新[我們貢獻的指導方針](https://github.com/MicrosoftDocs/windows-uwp/blob/docs/CONTRIBUTING.md)。 工作流程和對我們文件的外部貢獻的期望，可清楚說明這個新的指導方針。
DirectX 圖形 Infastructure (DXGI) | 遺失的 DXGI Api 已新增新的文件，並在 Windows 10 上呈現時，我們已提供最佳做法的相關文章。 </br> * [為獲得最佳效能，使用 DXGI 翻轉模型](https://docs.microsoft.com/windows/desktop/direct3ddxgi/for-best-performance--use-dxgi-flip-model)： 提供如何獲得最佳效能和效率現代的 Windows 版本上簡報堆疊中的指引。 </br> * [IDXGIOutput6::CheckHardwareCompositionSupport 方法](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/nf-dxgi1_6-idxgioutput6-checkhardwarecompositionsupport)： 會通知應用程式，支援的硬體自動縮放。 </br> * [DXGI_HARDWARE_COMPOSITION_SUPPORT_FLAGS 列舉](https://docs.microsoft.com/windows/desktop/api/dxgi1_6/ne-dxgi1_6-dxgi_hardware_composition_support_flags)： 描述支援哪些硬體組合的層級。
入門 | 我們[要開始使用](../get-started/index.md)的內容有已 revitalized 與新的主題，提供上如何開發新的 Windows 10 人員可能會完成下列常見工作的資訊與指導方針： </br> * [建構表單](../get-started/construct-form-learning-track.md) </br> * [顯示清單中的客戶](../get-started/display-customers-in-list-learning-track.md) </br> * [儲存和載入設定](../get-started/settings-learning-track.md) </br> * [使用的檔案](../get-started/fileio-learning-track.md)
地圖樣式表編輯器 | 使用新的[地圖樣式表編輯器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft?rtc=1#activetab=pivot:overviewtab)應用程式以互動方式來自訂您將新增到您的應用程式的地圖的外觀。
了解 Microsoft | 新的[Microsoft 學習網站](https://www.microsoft.com/learning/default.aspx)提供新的實機操作學習和訓練的機會，Microsoft 開發人員。 目前，Microsoft 學習提供訓練和 Microsoft 365、 Microsoft Azure、 Office 365 和 Windows Server 的認證。
記事本 | [已更新 [記事本]](http://aka.ms/ant-man)，新增縮放、 迴繞尋找/取代和支援的 Unix/Linux (LF) 和 Mac (CR) 行尾。
Project Rome | 現在， [Project Rome](https://docs.microsoft.com/windows/project-rome/)會提供一致的程式設計體驗跨支援的平台和 Sdk。 </br>  新的[Microsoft Graph 通知](https://developer.microsoft.com/graph/docs/concepts/notifications-concept-overview)使用專案 Rome 來提供您的應用程式的人為主、 跨平台通知平台。
螢幕剪 | 新的[URI 配置](../launch-resume/launch-screen-snipping.md)可讓您的應用程式以程式設計方式開啟新的剪取，或者剪取與草圖利用啟動應用程式特定的映像的註解。
傳統型應用程式中的 UWP 控制項 | Windows 10 現在可讓您在 WPF、 Windows Forms 和 c + + Win32 傳統型應用程式中使用 UWP 控制項。 這表示您可以提升外觀、 感覺與您現有的傳統型應用程式與最新的 Windows 10 UI 功能只會透過 UWP 控制項，例如 Windows Ink 和 Fluent 設計系統支援的控制項可用的功能。 此功能稱為*XAML 群島*。 </br> 我們提供幾種方式可使用 XAML 群島在您的應用程式，根據您使用的應用程式平台。 WPF 和 Windows Form 應用程式可以使用一組[Windows 社群工具組](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中提供設計工具為導向開發經驗的控制項。 C + + Win32 應用程式必須使用*UWP XAML 裝載 API* [elementcompositionpreview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting)命名空間中。 如需詳細資訊，請參閱[傳統型應用程式中的 UWP 控制項](../xaml-platform/xaml-host-controls.md)。 </br> **注意：** Api 與控制項可讓 XAML 群島是目前可為開發人員預覽。 雖然我們鼓勵您嘗試它們在您自己的原型程式碼現在，我們不建議您使用它們在實際執行程式碼中這一次。
Windows 機器學習 | [Windows Machine Learning](https://docs.microsoft.com/windows/ai/)有現在正式啟動，提供功能，例如更快速評估及支援尖端機器學習模型。 若要支援想要將它整合到他們的應用程式開發人員，我們已建立新的文件網站使用數個全新和已更新的資源： </br> * [教學課程： 建立 Windows 機器學習傳統型應用程式 （c + +）](https://docs.microsoft.com/windows/ai/get-started-desktop)： 本教學課程示範如何建置適用於桌上型電腦的簡單 Windows ML 應用程式。 </br> * [教學課程： 建立 Windows 機器學習 UWP 應用程式 (C#)](https://docs.microsoft.com/windows/ai/get-started-uwp)： 在這個逐步教學課程中，使用 Windows ML 建立您的第一個 UWP 應用程式。 </br> * [Windows.AI.MachineLearning 命名空間](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning)： 最新版本的 Windows 10 SDK，已更新的 API 參考和開發人員現在可以使用此 API 適用於 UWP 和 Win32 應用程式。
Windows Mixed Reality | 開發人員現在可以要求硬體保護後端緩衝區紋理如果顯示器硬體，支援的允許應用程式使用像是 PlayReady 等來源的硬體保護的內容。 硬體保護支援和設定是可用的主要層使用新內容的[Windows.Graphics.Holographic.HolographicCamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera)，以及四邊形層，透過[Windows.Graphics.Holographic.HolographicQuadLayerUpdateParameters](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters)。

## <a name="iot-core"></a>IoT 核心版

功能 | 描述
 :------ | :------
AssignedAccessSettings | [\ [Assignedaccesssettings\] 類別](https://docs.microsoft.com/uwp/api/windows.system.userprofile.assignedaccesssettings)可讓呼叫不同的方法和屬性來存取使用者的受指派的存取權設定特定裝置。
預設應用程式概觀 | [Windows 10 IoT 核心版預設應用程式](https://docs.microsoft.com/windows/iot-core/develop-your-app/iotcoredefaultapp)已更新的新特色和功能，例如氣象，筆跡和音訊。
儀表板 | [Windows 10 Iot 核心版儀表板](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup)現在可讓開發人員使用 Dragonboard 410 C 或 NXP 閃爍自訂 FFUs 到他們的裝置上。
螢幕小鍵盤 | [螢幕小鍵盤適用於 IoT 裝置](https://docs.microsoft.com/windows/iot-core/develop-your-app/onscreenkeyboard)現在會使用相同的觸控式鍵盤元件作為 Windows 桌面版本。 這可讓功能，例如聽寫模式、 輸入法支援，以及一組完整的輸入範圍。
登入對話方塊的標題列 | Windows 10 IoT 核心版現在可設定[的系統對話方塊的標題列](https://docs.microsoft.com/windows/iot-core/develop-your-app/signindialogtitlebars)的選項。
觸控喚醒 | [喚醒觸控](https://docs.microsoft.com/windows/iot-core/learn-about-hardware/wakeontouch)可讓您的裝置螢幕關閉時不在使用中，同時快速開啟當使用者觸碰螢幕。 
Windows.System.Update | 新的[Windows.System.Update 命名空間](https://docs.microsoft.com/uwp/api/windows.system.update)可讓系統更新的互動式控制項。 此命名空間是僅適用於 Windows 10 IoT 核心版。

## <a name="web-development"></a>Web 開發

功能 | 描述
 :------ | :------
EdgeHTML 18 | Windows 10 年 10 月 2018年更新隨附[EdgeHTML 18](https://docs.microsoft.com/microsoft-edge/dev-guide)，最新的更新到 Microsoft Edge 瀏覽器和 JavaScript 引擎適用於 UWP app 使用。 EdgeHTML 18 帶來現代化並展開支援 Web 驗證 API、 新 WebView 控制項功能，以及更多 ！ 工具一邊 EdgeHTML 18 為帶來了新的 WebDriver 功能和自動更新，並增強功能 Edge DevTools 和 Edge DevTools 通訊協定。 查看[什麼是 EdgeHTML 18 的新功能](https://docs.microsoft.com/microsoft-edge/dev-guide)和[DevTools 在最新的 Windows 10 更新 (EdgeHTML 18)](https://docs.microsoft.com/microsoft-edge/devtools-guide/whats-new) ，如所有的詳細資料。
漸進式 Web 應用程式 | Windows 10 的 JavaScript app ( *WWAHost.exe*處理序中執行的 web app) 現在支援啟動之前的任何檢視，則會啟用選用[每個應用程式背景指令碼](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide#progressive-web-apps)和程序的期間執行。 這，您可以監視和修改瀏覽、 瀏覽跨追蹤狀態、 監視瀏覽錯誤，並檢視，則會啟用之前執行的程式碼。 當指定為[`StartPage`](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema2010-v2/element-application)在您[的應用程式資訊清單](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest)，每個應用程式的檢視 (windows) 皆已公開給指令碼的新執行個體為[`WebUIView`](https://docs.microsoft.com/en-us/uwp/api/windows.ui.webui.webuiview)類別，為一般的 (Win32) [WebView](https://docs.microsoft.com/en-us/uwp/api/windows.web.ui.iwebviewcontrol)提供相同的事件、 屬性和方法。
Web API 擴充功能 | 已新增的[舊版 Microsoft API 延伸](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)清單跨瀏覽器 web 開發的 Mozilla Developer Network 文件。 這些 API 的擴充功能都是唯一的 Internet Explorer 或 Microsoft Edge，並補充現有 MDN web 文件中的相容性及 broswer 支援的相關資訊。舊版 Microsoft [CSS 擴充功能](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)與[JavaScript 延伸模組](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也使用，且您可以找到豐富 web API 的資訊從 MDN 直接在呈現[Visual Studio Code。](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)
WebVR | 我們的[WebVR 開發人員指南](https://docs.microsoft.com/microsoft-edge/webvr/)，包括完整的首頁頁面重新設計和重新組織的目錄進行重大更新。 我們也已撰寫數個新的主題，包括： </br> * [什麼是 WebVR？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr) 說明 WebVR 的是，為什麼您應該使用它，以及如何開始針對它進行開發。 </br> * [漸進式 Web 應用程式中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas)： 了解如何新增 WebVR 至漸進式 Web 應用程式 (PWA)。 </br> * [WebView 中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview)： 了解如何新增 WebVR 至 WebView 控制項，在 Windows 10 的應用程式。 </br> * [WebVR 示範](https://docs.microsoft.com/microsoft-edge/webvr/demos)： 請查看某些 WebVR 示範使用 Microsoft Edge 與 Windows Mixed Reality 沉浸式頭戴式裝置。

## <a name="publish--monetize-windows-apps"></a>發佈 Windows 應用程式以及從中獲利

功能 | 描述
 :------ | :------
MSIX | [MSIX](https://docs.microsoft.com/windows/msix/overview)是提供現代化的封裝體驗，以所有 Windows 應用程式的新 Windows 應用程式套件格式。 開放原始碼 MSIX 格式啟用現代化部署功能同時保留現有的套件的功能。
MSIX 封裝工具 | 新的[MSIX 封裝工具](https://docs.microsoft.com/windows/msix/mpt-overview)） 可讓您重新封裝 MSIX 格式，您現有的傳統型應用程式，即使您不需要存取其原始碼。 它可以在命令列，或透過其互動式 UI 執行。
MSIX 的傳統型應用程式轉換器支援 | 您可以使用[Desktop App Converter](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)輸出 MSIX 套件中，使用`-MakeMSIX`參數。
MSIX MakeAppx.exe 工具支援 | 您可以使用 MakeAppx.exe 工具建立 MSIX 套件適用於 UWP app 或傳統桌面應用程式。 此工具包含在 Windows 10 SDK，而且可以從命令提示字元或指令碼檔案使用。 </br> 對於 UWP 應用程式，請參閱 <<c0>建立應用程式套件使用 MakeAppx.exe 工具。 </br> 對於傳統型應用程式，請參閱[手動封裝的傳統型應用程式](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)。
套件支援的架構 | [套件支援的架構](https://docs.microsoft.com/windows/msix/package-support-framework-overview)是開放原始碼套件，可協助您將套用修正程式到您現有的傳統型應用程式時您不需要的存取權的原始程式碼，讓它可以 MSIX 容器中執行。
Microsoft store 分析 API | [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md)現在包含下列的新方法： </br> * [取得您的 UWP 應用程式的深入解析資料](../monetize/get-insights-data-for-your-app.md) </br> * [取得傳統型應用程式的深入解析資料](../monetize/get-insights-data-for-your-desktop-app.md) </br>* [取得傳統型應用程式的升級區塊](../monetize/get-desktop-block-data.md) </br> * [取得傳統型應用程式的升級區塊詳細資料](../monetize/get-desktop-block-data-details.md)

## <a name="videos"></a>影片

下列影片自 Fall Creators Update 發行後即已發佈，重點說明 Windows 10 中適用於開發人員的新功能及改良功能。

### <a name="cwinrt"></a>C++/WinRT

C + + /winrt 是以新的方式撰寫和使用 Windows 執行階段 Api。 它僅在標頭檔案中實作以及設計用來提供您現代化應用程式功能的第一級存取。 若要了解它的運作方式，然後[閱讀開發人員文件](../cpp-and-winrt-apis/index.md)如需詳細資訊，[請觀看影片](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be)。

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>開始使用適用於開發人員： 建立和自訂 Windows 10 上的表單

我們[要開始使用的文件](../get-started/index.md)給 Windows 開發人員現在提供基本的應用程式開發工作的實機操作體驗。 這段影片會逐步引導您透過其中一個這些主題中，並說明在您的應用程式中建立表單的 UI 的基本知識。 [觀看影片](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)以查看程式碼的動作，然後[自行查看本主題。](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>增強您使用專案特質聊天 Bot

特質聊天專案可讓您將可自訂的角色新增到您的聊天 bot。 整合至 Microsoft Bot 架構 SDK，您可以新增更交談的方式與客戶互動的小型通話功能。 若要了解如何實作它，然後[嘗試互動示範](http://aka.ms/PersonalityChat)實機操作體驗，[觀看影片](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)。

### <a name="multi-instance-uwp-apps"></a>多執行個體 UWP app

Windows 現在可讓您與它自己的個別處理程序中的每個執行您的 UWP 應用程式的多重執行個體。 若要了解如何建立新的應用程式更多的指導方針，說明如何針對支援此功能，然後[閱讀開發人員文件](../launch-resume/multi-instance-uwp.md)，以及為什麼要使用此功能，[請觀看影片](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be)。

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 外掛程式

Unity 的 Xbox Live 外掛程式包含加入 Xbox Live 簽署、 統計資料、 朋友清單、 雲端儲存空間及排行榜加入您的遊戲的支援。 [觀看影片](https://youtu.be/fVQZ-YgwNpY)以了解更多]，然後[下載 GitHub 套件](https://aka.ms/UnityPlugin)來開始。

### <a name="one-dev-question"></a>一個開發人員問題

在開發人員問題影片系列，longtime Microsoft 開發人員會討論一系列的 Windows 開發、 小組文化特性和歷程記錄的相關問題。

* [Raymond Chen Windows 開發和歷程記錄](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [在 Windows 開發和歷程記錄 Larry Osterman](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson 上漸進式 Web 應用程式](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann 上 webhint 工具](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)

## <a name="samples"></a>範例

### <a name="customer-orders-database"></a>客戶訂單資料庫

[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)已經更新，使用新的控制項，例如[資料格](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid)、 [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)及[展開工具](https://docs.microsoft.com/windows/communitytoolkit/controls/expander)。

### <a name="customer-database-tutorial"></a>客戶資料庫教學課程

[客戶資料庫教學課程](../enterprise/customer-database-tutorial.md)建立基本的 UWP 應用程式，用於管理的客戶清單，並引進了概念和企業開發有用的作法。 它會引導您完成實作 UI 元素以及新增對本機 SQLite 資料庫，作業，並提供連線到遠端的其餘部分資料庫，如果您想要進一步的鬆散指導方針。

### <a name="photo-editor-cwinrt"></a>Photo Editor C + + /winrt

[Photo Editor 範例應用程式](https://github.com/Microsoft/Windows-appsample-photo-editor)展示使用開發[C + + /winrt](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)語言投影。 應用程式可讓您從**圖片**媒體櫃，擷取相片，然後選擇以編輯影像相關聯的相片效果。

### <a name="windows-machine-learning"></a>Windows 機器學習

[Windows 機器學習](https://github.com/Microsoft/Windows-Machine-Learning)存放庫已更新至最新的 Windows 10 sdk，運作，並包含以 C#、 c + + 和 JavaScript 撰寫的範例。

### <a name="xaml-hosting-api"></a>XAML 裝載 API

[XAML 裝載 API 範例](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)是使用 UWP XAML 裝載 （也稱為 XAML 群島） API 的各種的案例會反白顯示的 Win32 傳統型應用程式。 在專案中，納入了 Windows Ink、 媒體播放程式和瀏覽檢視控制項中的資源庫樣式簡報。 外部的一般控制項使用方式，此範例也示範處理 XAML 與原生 Windows 事件/訊息，然後將基本的 XAML 資料繫結。
