---
title: 適用於開發人員的 Windows 10 的新功能
description: Windows 10 組建 18362 與新的開發人員工具提供由通用 Windows 平台所提供的工具、功能及體驗。
keywords: 新功能, 新增功能, 更新, 多項更新, 功能, 新, Windows 10, 最新, 開發人員, 18362, 5 月
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 4e92afd112ce7600bcfa650e0bb3bbeffabd7bd0
ms.sourcegitcommit: f120968069702a7210756b508dabc4a1a8c20d53
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2019
ms.locfileid: "72438224"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>適用於開發人員的 Windows 10 (組建 18362) 最新動向

Windows 10 組建 18362 (也就是 SDK 版本 1903年)，搭配使用 Visual Studio 2019，提供工具、 功能和體驗，以建立引人注目的 Windows 應用程式。 在 Windows 10 上[安裝工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows app](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

這是此版本中 Windows 開發人員會感興趣的新功能和改良功能以及指引的集合。 如需新增到 Windows SDK 之新命名空間的完整清單，請參閱 [Windows 10 組建 18362 API 變更](windows-10-build-18362-api-diff.md)。 如需 Windows 10 重點功能的詳細資訊，請參閱 [Windows 10 中有哪些酷功能](https://go.microsoft.com/fwlink/?LinkId=823181)。

## <a name="design--ui"></a>設計與 UI

功能 | 描述
:------ | :------
AnimatedVisualPlayer | [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer) API 可在您的應用程式中裝載和控制動畫視覺效果的播放。 此 API 用來控制和顯示 [Lottie](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie) 視覺效果之類的內容，可讓您呈現您應用程式中原生的 Adobe AfterEffects 動畫。
CompactDensity | 在您的應用程式中啟用[精簡模式](../design/style/spacing.md)，可啟用密集、資訊豐富的控制項群組。 這可以協助瀏覽大量內容、將頁面上可見的內容最大化，或在使用者使用指標輸入時協助瀏覽和互動。
ItemsRepeater | [ItemsRepeater](../design/controls-and-patterns/items-repeater.md) 控制項可以建立自訂體驗，以便對使用者顯示集合。 ItemsRepeater 未提供完整的使用者體驗或預設 UI。 然而，它是一個建置組塊，您可用來建立自己的唯一集合型體驗和自訂控制項。
教學提示 | [教學提示](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)是半持續性的內容豐富飛出視窗，可提供內容資訊。 您可以將此控制項用於通知、提醒及教導使用者有關新的或重要功能。
UI 命令 | 透過 [UWP 應用程式中的命令](../design/controls-and-patterns/commanding.md)，使用 [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 和 [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) 類別 (連同 ICommand 介面) 來共用及管理橫跨各種控制項類型的命令 (不論所使用的裝置和輸入類型為何)。
Windows UI 程式庫 | Windows UI 程式庫的最新官方版本 – [WinUI 2.1](https://docs.microsoft.com/uwp/toolkits/winui/release-notes/winui-2.1) – 為您的 Windows 應用程式提供活躍的 XAML 控制項。 WinUI 程式庫 API 可在較舊版本的 Windows 10 上執行，因此您並不需要加入版本檢查或條件式 XAML 來支援沒有使用最新作業系統的使用者。
傳統型應用程式中的視覺層 | 您現在可以[使用傳統型應用程式中的 UWP 視覺層 API](../composition/visual-layer-in-desktop-apps.md)。 這些 API 提供高效能、保留模式的 API 來處理圖形、效果和動畫，而且是各種 Windows 裝置的 UI 基礎。
Z 深度和陰影 | 使用 [Z 深度和陰影](../design/layout/depth-shadow.md)在 UWP 應用程式中建立提高權限。 這些新功能可讓您的應用程式 UI 更容易進行掃描，且進一步傳達使用者要聚焦的最重要資訊。

## <a name="develop-windows-apps"></a>開發 Windows 應用程式

功能 | 描述
:------ | :------
反惡意程式碼掃描介面 (AMSI) | 了解[反惡意程式碼掃描介面 (AMSI) 如何協助您抵禦惡意程式碼](https://docs.microsoft.com/windows/desktop/amsi/how-amsi-helps)，然後查看[範例程式碼](https://docs.microsoft.com/windows/desktop/amsi/dev-audience)以了解如何在傳統型應用程式中加以實作。
C++/WinRT 2.0 | 已發行 2.0 版的 C++/WinRT。 請查看[C++/WinRT 新增功能](../cpp-and-winrt-apis/news.md)，以取得所有新變更和新增項目的完整流程表。
選擇您的平台 | 想要建立新的傳統型應用程式？ 請查看我們改造的[選擇您的平台](https://docs.microsoft.com/windows/desktop/choose-your-technology)頁面，以取得 UWP、WPF 和 Windows Forms 平台的詳細描述和比較，以及 Win32 API 的進一步資訊。
交談式代理程式 | [Windows.ApplicationModel.ConversationalAgent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent) 命名空間可讓您將 Windows 平台代理程式啟用執行階段 (AAR) 所支援的任何數位助理新增至 Windows 應用程式。
雲端檔案 API | 「雲端檔案 API」  可讓您[建置支援預留位置檔案的雲端同步引擎](https://docs.microsoft.com/windows/desktop/cfapi/build-a-cloud-file-sync-engine)。
Direct 3D 12 | 之外，[Direct3D 12 轉譯行程](/windows/desktop/direct3d12/direct3d-12-render-passes)可以改善您的轉譯器效能 (如果它是以並排式延遲轉譯 (TBDR) 為基礎的話)。 此技術可讓您的應用程式更容易識別資源轉譯順序需求和資料相依性，進而協助您的轉譯器改善 GPU 效率。 這會減少往返晶片記憶體的記憶體流量。
Direct Machine Learning (DirectML) | [DirectML](https://docs.microsoft.com/windows/desktop/direct3d12/dml) 是適用於機器學習的低階硬體加速 API。 它具有 DirectX 12 樣式的熟悉 (原生C++，nano-COM) 程式設計介面和工作流程。 您可以將機器學習推斷工作負載整合到您的遊戲、引擎、中介軟體、後端或其他應用程式中。 所有 DirectX 12 相容硬體都支援 DirectML。
DirectX HLSL | [HLSL Shader Model 6.4](https://docs.microsoft.com/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12)提供可搭配 DirectML 使用得新機器學習 intrinsics。
驅動程式開發 | 已針對 Windows 驅動程式開發人員增加新的音訊、相機、顯示、網路、行動寬頻、列印、感應器、儲存體及 wifi 功能。 如需進一步詳細資訊，請查看[驅動程式開發新增功能](https://docs.microsoft.com/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest)。
檔案系統作業 | 此[最佳做法指南](../files/best-practices-for-writing-to-files.md)可協助您充分利用 Windows.Storage.FileIO 和 Windows.Storage.PathIO 類別來執行檔案系統 I/O 作業。
遊戲台與遙控器的互動 | 使用[遊戲台和遠端控制互動](../design/input/gamepad-and-remote-interactions.md)，打造可使用且可存取的互動體驗。 透過這些互動，您的應用程式在十英呎遠的地方可如同兩英呎遠般以直覺方式輕鬆使用。
日本年號變更 | 我們提供了[這些指示](../design/globalizing/japanese-era-change.md)，說明如何確保您的 Windows 應用程式已準備好因應 2019 年 5 月 1 日起生效的日本年號變更。 [此頁面也會以日文提供](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)。
WPF、Windows Forms 和 WinUI 的開放原始碼 | WPF、Windows Forms 和 WinUI UX 架構現在可用於在 GitHub 上貢獻開放原始碼。 如需詳細資訊和連結，請參閱[建置 Windows 應用程式部落格](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)。
適用於 Xbox 的漸進式 Web 應用程式 | 使用[適用於 Xbox One 的漸進式 Web 應用程式](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)，您可以擴充 Web 應用程式並讓它以 Xbox One 應用程式的形式透過 Microsoft Store 提供，同時仍繼續使用您現有的架構、CDN 及伺服器後端。 在大部分的情況下，您可使用 Windows 中的相同方式針對 Xbox One 封裝您的 PWA。 本指南將逐步引導您完成此程序，並強調主要差異。
Project Rome | Project Rome SDK 現在適用於 Android 和 iOS。 了解如何整合圖形通知與每個平台：[Android](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-android) 和 [iOS](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-ios)。
遠端相機 | 使用 DeviceWatcher 類別，[連線到遠端相機](../audio-video-camera/connect-to-remote-cameras.md)，並將這些相機中的框架讀到 Windows 應用程式中。
傳統型應用程式中的 UWP 控制項 (XAML Island) | Windows SDK 中用於在 WPF、Windows Forms 和 C++ Win32 傳統型應用程式中裝載 UWP 控制項的 API，不再位於開發人員預覽版中。 如需詳細資訊，請參閱[傳統型應用程式中的 UWP 控制項](../xaml-platform/xaml-host-controls.md)。
Visual Studio 2019 | 已發行 Visual Studio 2019，內含適用於任何開發人員、應用程式或平台的最新工具和服務。 請查看 [Visual Studio 2019 新增功能](https://docs.microsoft.com/visualstudio/ide/whats-new-visual-studio-2019?view=vs-2019)以了解最新資訊並開始使用。
Win32 WebView | 我們的[常見問題集](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)會提供在傳統型應用程式中使用 Microsoft Edge WebView 時的常見問題解答，以及連範例和其他資源的連結。
Windows 命令列 | [新的主控台功能](https://devblogs.microsoft.com/commandline/new-experimental-console-features/)包括實驗性 [終端機] 索引標籤，內含捲動、游標形狀和游標色彩的設定。 深入了解[適用於開發人員的 Windows 命令列工具部落格](https://devblogs.microsoft.com/commandline/)。
Windows 社群工具組 | Windows 社群工具組 v5.1 為動畫、遠端裝置、影像裁剪和協助工具提供令人興奮的更新。 </br> • 新的 [文件庫](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie) 利用 Windows.UI.Composition API 在 Windows 10 (1809) 上提供高品質動畫支援，並允許使用 [Bodymovin](https://aescripts.com/bodymovin/) JSON 檔案或最佳化程式碼產生的類別在 Windows 應用程式中播放。 試用來自 Microsoft Store 的新 [Lottie 檢視器應用程式](https://aka.ms/lottieviewer)來測試動畫，並為您的 Windows 應用程式產生最佳化程式碼。 </br> • 新的[遠端裝置選擇器](https://docs.microsoft.com/windows/communitytoolkit/controls/remotedevicepicker)可讓使用者選取裝置 (就近或可存取雲端)、啟動該裝置上的應用程式，或與遠端裝置上的應用程式服務通訊。 </br> • 新的 [ImageCropper 控制項](https://docs.microsoft.com/windows/communitytoolkit/controls/imagecropper)整合裁剪功能，以便選取設定檔圖片或使用相片編輯工具。 </br> • 此外，還有控制項的存取性改進、適用於 WPF 和 WinForms 的 [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0 預覽套件更新，以及您可在[版本資訊](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0)中了解的其他功能。
Windows Machine Learning | 我們已重新設計 Windows AI 文件，將其劃分成三個領域：Windows Machine Learning (WinML)、Windows Vision Skills 和 Direct Machine Learning (DirectML)。 請查看新的[登陸頁面](https://docs.microsoft.com/windows/ai/) </br> • Visual Studio 中的 [*MLGen* 體驗](https://docs.microsoft.com/windows/ai/mlgen)持續變更。 在 Windows 10 版本 1903 和更新版本中，*mlgen* 不再包含於 Windows 10 SDK 中。 如果您使用 VS 2017，則應該改為下載並安裝 Visual Studio 擴充功能 [Windows Machine Learning 程式碼產生器 VS 2017](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen)。 如果您使用 Visual Studio 2019，則應該安裝 [Windows Machine Learning 程式碼產生器](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2)擴充功能。 </br> • 我們也很榮幸宣布新的權數封裝支援。 開發人員現在可以使用稱為權數封裝 (透過 [WinMLTools 轉換器](https://docs.microsoft.com/windows/ai/convert-model-winmltools)提供) 的技術，降低其 ML 模型的磁碟使用量。
WinRT 彙總參考 | 我們已新增 [WinRT 類型系統](https://docs.microsoft.com/uwp/winrt-cref/winrt-type-system)和 [WinMD 檔案](https://docs.microsoft.com/uwp/winrt-cref/winmd-files)的完整說明，以提供關於 WinRT API 結構定義的特定深入注意事項。
適用於 Linux 的 Windows 子系統 (WSL) | [WSL 的近期更新](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/)包括能夠使用 [檔案總管] 從 Windows 存取 Linux 檔案，以及 wsl.exe 和 wslconfig.exe 的一些新命令。
Windows Vision Skills | [Windows Vision Skills](https://docs.microsoft.com/windows/ai/windows-vision-skills) 是一組 API，可讓您建立臉部辨識等一些「技能」，然後將其封裝為其他應用程式可取用的 NuGet 套件，甚至不需包含機器學習模型。

## <a name="publish--monetize-windows-apps"></a>發佈 Windows 應用程式以及從中獲利

功能 | 描述
 :------ | :------
MSIX | [Windows 10 組建 1709 和 1803 上的 MSIX 支援](https://docs.microsoft.com/windows/msix/msix-1709-and-1803-support)說明 Windows 10 版本 1809 之前的版本支援那些 MSIX 功能。
MSIX 封裝和部署 | 我們引進了數個[修改套件相關改進功能](https://docs.microsoft.com/windows/msix/modification-package-insider-preview-build-18312)，讓您更輕鬆地在 MSIX 套件中封裝自訂項目。 這些改進功能包括套件資訊清單中的新 **rescap6:ModificationPackage** 元素、能夠利用修改套件覆寫主要套建中的檔案，以及能夠將檔案系統型外掛程式封裝為 MSIX 修改套件。
MSIX 封裝工具 | • 我們已新增[在遠端電腦上執行轉換的支援](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup)。 我們也引進了 [MSIX 封裝工具測試人員計畫](https://docs.microsoft.com/windows/msix/packaging-tool/insider-program)，以供提早存取新的工具功能。 </br> • [1709 和更新版本上的 MSIX 套件支援](https://docs.microsoft.com/windows/msix/packaging-tool/support-on-1709-and-later)專為 Windows 10 版本 1709 和 1803，提供使用 MSIX 封裝工具建置套件的相關指引。 </br> • [Hyper-V 快速建立上的 MSIX 封裝環境](https://docs.microsoft.com/windows/msix/packaging-tool/quick-create-vm)說明如何建立適用於 MSIX 封裝專案的虛擬環境。 </br> • [搭售 MSIX 封裝](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages)提供使用 MSIX 封裝工具建立套件組合的指示。 </br> • [Windows 10 版本 1809 上的修改套件](https://docs.microsoft.com/windows/msix/modification-package-1809-update)包含使用 MSIX 封裝工具和 MakeApp.exe 1809 為 Windows 10 版本 1809 和更新版本建立修改套件的指示。
MSIX SDK | [使用 MSIX SDK 來建置跨平台使用的套件](https://docs.microsoft.com/windows/msix/msix-sdk/sdk-guidance)，並了解如何指定您希望您的套件解壓縮的目標平台。

## <a name="microsoft-learn"></a>Microsoft Learn

Microsoft Learn 為 Microsoft 開發人員提供新的實際操作學習和訓練機會。

* 如果您有興趣了解如何開發 Windows 應用程式，請參閱[我們新的學習路徑](https://docs.microsoft.com/learn/paths/develop-windows10-apps/)，以取得平台、工具，以及如何撰寫前幾個應用程式的完整介紹。

* 想要了解如何將 UI 功能新增至您的 Windows 應用程式嗎？ 了解如何[建立 UI](https://docs.microsoft.com/learn/modules/create-ui-for-windows-10-apps/)、[將瀏覽和媒體新增至您的 UI](https://docs.microsoft.com/learn/modules/enhance-ui-of-windows-10-app/)，或[實作資料繫結](https://docs.microsoft.com/learn/modules/implement-data-binding-in-windows-10-app/)。

* 如果您想了解 Web 開發，請查看[使用 Visual Studio Code 開發 Web 應用程式](https://docs.microsoft.com/learn/modules/develop-web-apps-with-vs-code/)或[建置簡單的網站](https://docs.microsoft.com/learn/modules/build-simple-website/)。

* 或者，隨意瀏覽 [Microsoft Learn 上的所有 Windows 開發人員模組](https://docs.microsoft.com/learn/browse/?products=windows&resource_type=module)。

## <a name="videos"></a>影片

### <a name="progressive-web-apps"></a>漸進式 Web 應用程式

漸進式 Web 應用程式是一個網站，其作用如同橫跨不同瀏覽器和各種 Windows 10 裝置的原生應用程式。 [觀看影片](https://youtu.be/ugAewC3308Y)進一步了解，然後[查看文件](https://aka.ms/Windows-PWA)以開始使用。

### <a name="vs-code-series"></a>VS Code 系列

請查看 [Visual Studio Code 上新的影片系列](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)，以取得何謂 VSCode、其使用方式，以及其建立方式的相關資訊。

### <a name="mixed-reality-services"></a>混合實境服務

最近已宣布 HoloLens 2。 請查看[混合實境的這一系列影片](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST)，以取得最新資訊，以及如何參與並開始開發。

### <a name="one-dev-question"></a>One Dev Question

在 One Dev Question 影片系列中，資深的 Microsoft 開發人員會談論一系列關於 Windows 開發、團隊文化和歷史的問題。

* [Raymond Chen：關於 Windows 開發與歷史](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman：關於 Windows 開發與歷史](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson：漸進式 Web 應用程式](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann：webhint 工具](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)
