---
title: Windows 10 (組建 19041) 的新功能
description: Windows 10 組建 18362 與新的開發人員工具提供由 Windows 10 所提供的工具、功能及體驗。
keywords: 新功能, 新增功能, Windows, Windows 10, 更新, 多項更新, 功能, 新, 最新, 開發人員, 19041, 5 月
ms.date: 05/12/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 63a6138841a19629523b452eab2f3e7b5d125c37
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174422"
---
# <a name="whats-new-for-developers-in-windows-10-build-19041"></a>Windows 10 提供給開發人員的新功能 (組建 19041)

Windows 10 組建 19041 (也稱為 SDK 2004 版) 結合 Visual Studio 2019 和相關工具及功能，提供您建立優異 Windows 應用程式所需的所有項目。 在 Windows 10 上[安裝工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank) 之後，就表示您已經準備好[建立新的通用 Windows 應用程式](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的應用程式程式碼](../porting/index.md)。

這是此版本中 Windows 開發人員會感興趣的新功能和改良功能以及指引的集合。 如需新增到 Windows SDK 之新命名空間的完整清單，請參閱 [Windows 10 組建 19041 API 變更](windows-10-build-19041-api-diff.md)。 如需 Windows 10 重點功能的詳細資訊，請參閱 [Windows 10 中有哪些酷功能](https://developer.microsoft.com/windows/windows-10-for-developers)。

## <a name="windows-10-apps"></a>Windows 10 應用程式

功能 | 說明
:------ | :------
Bluetooth 音訊播放 | [啟用來自遠端藍牙連線裝置的音訊播放](../audio-video-camera/enable-remote-audio-playback.md)示範如何使用 [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) 來啟用藍牙連線的遠端裝置，以在本機電腦上播放音訊，以便將電腦設定成藍牙喇叭，並允許使用者聽到來自手機的音訊。
C# 應用程式移轉 | 我們已記載將 C# 應用程式移轉到 C++/WinRT 的程序。 [將剪貼簿範例從 C# 移轉到 C++/WinRT](../cpp-and-winrt-apis/clipboard-to-winrt-from-csharp.md) 內容相關，且是根據特定實際移轉體驗進行。 其相關主題[從 C# 移至 C++/WinRT](../cpp-and-winrt-apis/move-to-winrt-from-csharp.md) 是以更精闢的方式探討技術詳細資料和涉及移轉的步驟。 
C++/WinRT | 在[近期改進/新增彙總](../cpp-and-winrt-apis/news.md#rollup-of-recent-improvementsadditions-as-of-march-2020)中，閱讀關於組建時間和執行時間效能改進 (搭配 Visual C++ 編譯器小組達成) 的 C++/WinRT 更新。 </br> 針對 C++/WinRT，我們已將更多資訊新增至下列主題：[從 C++/CX 移轉](../cpp-and-winrt-apis/move-to-winrt-from-cx.md#boxing-and-unboxing)、[從 C# 移轉](../cpp-and-winrt-apis/move-to-winrt-from-csharp.md#boxing-and-unboxing)、[簡單的 C++/WinRT Windows UI 程式庫範例](../cpp-and-winrt-apis/simple-winui-example.md)、[並行](../cpp-and-winrt-apis/concurrency.md)、[get_unknown()](/uwp/cpp-ref-for-winrt/get-unknown)，以及[使用 C++/WinRT 的 XAML 自訂 (範本化) 控制項](../cpp-and-winrt-apis/xaml-cust-ctrl.md)。
DirectX | 我們針對數個過去的 Windows 版本，引進了數個與 DirectX 相關的「新功能」主題，從「建立者」更新到 Windows 10 (1903 版)。 [DirectWrite 的新功能](/windows/win32/directwrite/what-s-new-in-directwrite-for-windows-8-consumer-preview)、[DXGI 1.6 增強功能](/windows/win32/direct3ddxgi/dxgi-1-6-improvements)，以及 [Direct3D 12 的新功能](/windows/win32/direct3d12/new-releases)。
DirectXMath | 我們發佈了 21 個新的 DirectXMath 主題，涵蓋兩個矩陣結構及其成員函式和 free 函式。 範例為 [XMFLOAT3X4 結構](/windows/win32/api/directxmath/ns-directxmath-xmfloat3x4)。
Direct3D | [使用 DirectX 搭配高動態範圍顯示器和進階色彩](/windows/win32/direct3darticles/high-dynamic-range)提供 Windows high-dynamic-rnge 應用程式的最佳做法清單。 </br> 新的 [ID3D11On12Device2](/windows/win32/api/d3d11on12/nn-d3d11on12-id3d11on12device2) 介面及其方法，讓您可取得透過 Direct3D 11 API 建立的資源，並在 Direct3D 12 中使用。
Direct3D 12 | 已新增 [Direct3D 12 Core 1.0 功能層級](/windows/win32/direct3d12/core-feature-levels)，供僅限計算的裝置使用。 </br> 已針對 [ID3D12Debug3 介面](/windows/win32/api/d3d12sdklayers/nn-d3d12sdklayers-id3d12debug3)新增主題。
Direct ML | 已將 18 個運算子新增至 DirectML，這是在其中建置 WinML 的低層級硬體加速 API。 例如，[DML_ACTIVATION_SHRINK_OPERATOR_DESC 結構](/windows/win32/api/directml/ns-directml-dml_activation_shrink_operator_desc)。
錯誤報告 | RoFailFastWithErrorContextInternal2 函式已新增至 Win32，會引發例外狀況，其中可能包含額外的錯誤內容。
機器學習 | Windows Machine Learning [現在支援 ONNX 1.4 版和 opset 9](/windows/ai/windows-ml/release-notes)。 </br>  [CloseModelOnSessionCreation](/uwp/api/windows.ai.machinelearning.learningmodelsessionoptions.closemodelonsessioncreation?view=winrt-19041) API 可讓您在不再需要學習模型後，自動將其關閉來節省記憶體。
Wi-Fi | 已新增數個新的原生 WiFi 函式和結構，例如 [WlanDeviceServiceCommand 函式](/windows/win32/api/wlanapi/nf-wlanapi-wlandeviceservicecommand)。
Wi-Fi 熱點 2 | [透過網站佈建 Wi-Fi 設定檔](/windows/win32/nativewifi/prov-wifi-profile-via-website) 描述 Wi-Fi 熱點 2 的新功能。
Windows 全像攝影版 Interop | 已新增 [`windows.graphics.holographic.interop.h`](/windows/win32/api/windows.graphics.holographic.interop) 標頭，包含 17 個 Win32 API。 API 可用於 Win32 與 Windows 執行階段之間的交互作業。 在 Windows 10 組建 18362 中新增 API 時，此為組建 19041 的新標頭。
Windows Sockets | 已對 Windows Socket 2 SPI 內容進行增強。 我們所改善和增強的其中一個主題範例是 [LPWSPEVENTSELECT 回呼函式](/windows/win32/api/ws2spi/nc-ws2spi-lpwspeventselect)主題。
XAML Islands - 基礎 | 使用 XAML Islands 在您的傳統型 Windows 應用程式中裝載 UWP XAML 控制項。 了解如何[在 WPF 應用程式中裝載標準 UWP 控制項](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands)以及[在 C++ Win32 應用程式中裝載標準 UWP 控制項](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands-cpp)。
XAML Islands - 自訂控制項 | [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) 和 [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet 套件可讓您更輕鬆地在 .NET 和 C++ Win32 應用程式中裝載自訂 UWP XAML 控制項。 </br> 如需逐步解說，請參閱[在 WPF 應用程式中裝載自訂 UWP 控制項](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands)以及[在 C++ Win32 應用程式中裝載自訂 UWP 控制項](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands-cpp)。 </br> 最後，如需更複雜的 C++ Win32 案例相關指引，請參閱 [XAML Islands 的進階案例](/windows/apps/desktop/modernize/advanced-scenarios-xaml-islands-cpp)。

## <a name="build-with-windows"></a>使用 Windows 裝置

功能 | 說明
:------ | :------
Windows 開發環境 | [Windows 開發環境](/windows/dev-environment/)文件提供的資源，可讓您使用 Windows 在各種平台上進行開發，以達成您可能進行的任何開發目標。
Windows 上的 Python | [Windows 上的 Python](/windows/python/) 一節針對 Python 語言的新手開發人員，以及想要使用 Windows 上提供的其他工具來最佳化其 Python 開發的開發人員提供資訊。 了解如何設定 [Web 開發](/windows/python/web-frameworks)以及[資料庫互動](/windows/python/databases)的 Python 環境。
Windows 10 的 NodeJS | [適用於 Node.js 開發環境的建議設定](/windows/nodejs/setup-on-wsl2)針對要部署至 Linux 伺服器的進階開發人員提供詳細指導方針。 也可以使用適用於[熱門 Node.js Web 架構](/windows/nodejs/web-frameworks)、[資料庫互動](/windows/nodejs/databases)和 [Docker 容器](/windows/nodejs/containers)的設定指示。
Mac 到 Windows | 我們的[變更開發環境的指南](/windows/dev-environment/mac-to-windows)，適用於將開發平台從 Mac 轉換至 Windows 的使用者，並提供類似捷徑和開發公用程式的對應。
Windows 終端機 | [新式的終端機應用程式](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab)，適用於使用命令列工具和命令介面 (命令提示字元、PowerShell 和 Windows 子系統 Linux 版 (WSL)) 的使用者。 其主要功能包括多個索引標籤、窗格、Unicode 和 UTF-8 字元支援、GPU 加速文字轉譯引擎，以及讓您能夠建立自己的佈景主題並自訂文字、色彩、背景和快速鍵繫結。
WSL 2 | 現已提供[新版本的 Windows 子系統 Linux 版 (WSL)](/windows/wsl/wsl2-about)。 WSL 2 功能重新設定了架構，以在 Windows 上執行實際的 Linux 核心，可增加檔案系統效能，並新增完整的系統呼叫相容性。 這個新架構會變更 Linux 二進位檔與 Windows 和您電腦硬體的互動方式，但仍然提供與舊版 WSL 相同的使用者體驗。 每個個別的 Linux 散發套件都可以 WSL1 或 WSL2 散發版本的形式執行，可以並存執行，且可以隨時進行變更。 </br> [安裝 WSL 2](/windows/wsl/wsl2-install) 以開始使用。 </br> 深入探索關於 [WSL 1 與 WSL 2之間使用者體驗變更](/windows/wsl/wsl2-ux-changes)的詳細資訊。 </br> 查看[有關 WSL 2 的常見問題](/windows/wsl/wsl2-faq)。

## <a name="msix-packaging-and-deployment"></a>MSIX、封裝和部署

功能 | 說明
:------ | :------
MSIX | 自上次發行 Windows 10 SDK 以來，已對 [MSIX 封裝格式](/windows/msix/overview)進行重大更新。 
使用服務進行封裝 | MSIX 和 MSIX 封裝工具[現在支援包含服務的應用程式套件](/windows/msix/packaging-tool/convert-an-installer-with-services)。
MSIX 套件中的指令碼 | 您可以[使用套件支援架構 (PSF) 在 MSIX 應用程式套件中執行指令碼](/windows/msix/psf/run-scripts-with-package-support-framework)，讓 IT 專業人員在使用 MSIX 封裝後，將應用程式動態自訂到使用者的環境。
強制執行的封裝完整性 | 您現在可以使用封裝資訊清單中的 [uap10： PackageIntegrity 元素](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-packageintegrity)，針對 MSIX 封裝的內容強制執行封裝完整性。 當您透過 MSIX 封裝工具建立 MSIX 套件時，也可以強制執行封裝完整性。
疏鬆套件 | 您可建置*疏鬆套件*並向您的應用程式進行註冊，以[將套件識別資料授與未封裝在 MSIX 套件中的傳統型應用程式](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)。 此功能可讓尚無法採用 MSIX 封裝進行部署的傳統型應用程式，使用需要套件識別資料的 Windows 10 擴充性功能。
託管應用程式 | 您現在可以[建立託管應用程式](../launch-resume/hosted-apps.md)。 裝載的應用程式與父主機應用程式共用相同的可執行檔和定義，但其外觀和行為類似系統上的個別應用程式。 裝載的應用程式適用於您想要元件 (例如可執行檔或指令碼檔案) 行為類似獨立 Windows 10 應用程式，但元件需要主機處理序才能執行的情況。 裝載的應用程式可以有自己的開始磚、身分識別，以及與 Windows 10 功能 (例如背景工作、通知、磚和共用目標) 的深度整合。

## <a name="windows-ui-library-winui"></a>Windows UI 程式庫 (WinUI)

功能 | 說明
:------ | :------
WinUI 2.4 | [WinUI 2.4](/uwp/toolkits/winui/release-notes/winui-2.4)是 Windows UI 程式庫的最新公開發行版本。 所有版本的 WinUI 都可為您的 Windows 應用程式提供各種官方 UI 控制項，並提供做為與 Windows SDK 無關的 NuGet 套件，因此可以在舊版的 Windows 10 上執行。 [遵循這些指示](/uwp/toolkits/winui)安裝 WinUI。
RadialGradientBrush | WinUI 2.4 中的新功能，[RadialGradientBrush](../design/style/brushes.md#radial-gradient-brushes) 會在 Center、RadiusX 和 RadiusY 屬性所定義的橢圓形內繪製。 漸層的色彩會從橢圓形的中央開始，並在半徑上結束。
ProgressRing | WinUI 2.4 中的新功能，[ProgressRing 控制項](../design/controls-and-patterns/progress-controls.md)是用於強制回應的互動，直到 ProgressRing 消失之前，使用者都會被封鎖。 如果作業需要暫停與應用程式的大部分互動，直到作業完成為止，請使用此控制項。
TabView | [TabView 控制項](../design/controls-and-patterns/tab-view.md)的更新可讓您更充分掌控索引標籤的轉譯方式。 您可以設定未選取之索引標籤的寬度，並只顯示一個圖示來儲存螢幕空間，而且也可以隱藏未選取之索引標籤上的 [關閉] 按鈕，直到使用者將滑鼠停留在索引標籤上為止。
TextBox 控制項 | 啟用深色主題時，TextBox 系列控制項的背景色彩現在依預設會在文字插入時保持深色。 受影響的控制項包括 [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)、[RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)、[PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)、[可編輯的 ComboBox](/uwp/api/windows.ui.xaml.controls.combobox) 和 [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)。
NavigationView | [NavigationView 控制項](/uwp/api/microsoft.ui.xaml.controls.navigationview)現在支援階層式瀏覽，包括 Left、Top 和 LeftCompact 顯示模式。 階層式 NavigationView 適用於顯示頁面類別、識別具有相關子頁面的頁面，或是在有中樞樣式頁面連結到其他許多頁面的應用程式內使用。
Windows UI 程式庫 | XAML 控制項庫中提供每個 WinUI 功能的範例。 請在 [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)上下載，或[在 Github 上檢視原始程式碼](https://github.com/Microsoft/Xaml-Controls-Gallery)。
先前版本 | 自先前的 Windows 10 SDK 主要版本起，[WinUI 2.3](/uwp/toolkits/winui/release-notes/winui-2.3) 和 [WinUI 2.2](/uwp/toolkits/winui/release-notes/winui-2.2) 也已發行，為 Windows 開發人員提供進一步的新 UI 功能。

## <a name="samples"></a>範例

下列範例應用程式已更新為目標 Windows 10 組建 19041。

* [遠端工作階段 (測驗遊戲)](https://github.com/microsoft/Windows-appsample-remote-system-sessions)
* [客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
* [RSS 讀取程式](https://github.com/Microsoft/Windows-appsample-rssreader)
* [Marble Maze (滾珠迷宮)](https://github.com/Microsoft/Windows-appsample-marble-maze)
* [Photo Editor](https://github.com/Microsoft/Windows-appsample-photo-editor)
* [Lunch Scheduler (午餐排程器)](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
* [Coloring Book (著色本)](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Hue Light Controller (色調光源控制器)](https://github.com/Microsoft/Windows-appsample-huelightcontroller)
* [Photo Lab](https://github.com/Microsoft/Windows-appsample-photo-lab)
* [Family Notes (家庭記事本)](https://github.com/Microsoft/Windows-appsample-familynotes)

## <a name="videos"></a>視訊

### <a name="windows-terminal-the-secret-to-command-line-happiness"></a>Windows 終端機：命令列密碼的快樂秘密！

了解如何為您的工作流程自訂 Windows 終端機，並查看其功能運作方式的示範。 [查看影片](https://www.youtube.com/watch?v=2dsnwlnNBzs)，然後[閱讀文件](https://github.com/microsoft/terminal#terminal--console-overview)以取得詳細資訊。

### <a name="wsl2-code-faster-on-the-windows-subsystem-for-linux"></a>WSL2：在 Windows 子系統 Linux 版上更快速地撰寫程式碼

深入了解 WSL2 (新版本的 Windows 子系統 Linux 版)，以及針對改善效能所做的變更。 [查看影片](https://www.youtube.com/watch?v=MrZolfGm8Zk)，然後[閱讀文件](/windows/wsl/wsl2-about)以取得詳細資訊。

### <a name="msix-package-desktop-apps-for-windows-10-replace-outdated-installers"></a>MSIX：封裝適用於 Windows 10 的傳統型應用程式。 取代過時的安裝程式。

深入了解 MSIX，這是安裝 Windows 應用程式的套件格式，包括如何使用 Visual Studio 封裝現有的程式碼，以及如何部署和散發您的應用程式。 [查看影片](https://www.youtube.com/watch?v=yhOnClQrvBk)，然後[閱讀文件](/windows/msix/)以取得詳細資訊。