---
title: 適用於開發人員的 Windows 10 的新功能
description: Windows 10 組建 18362 和全新開發人員工具提供工具、 功能及由通用 Windows 平台提供的體驗。
keywords: 最新消息，功能新的更新，更新功能，新的 Windows 10 最新開發人員，18362，可能會
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 935d7f787d0cc23965c0fd51747b7687adb80a3f
ms.sourcegitcommit: a4fe508e62827a10471e2359e81e82132dc2ac5a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/03/2019
ms.locfileid: "66468320"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>What's New in Windows 10 開發人員建置 18362 時

Windows 10 組建 18362 (也就是 SDK 版本 1903年)，搭配使用 Visual Studio 2019，提供工具、 功能和體驗，以建立引人注目的 Windows 應用程式。 在 Windows 10 上[安裝工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows app](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

這是此版本中 Windows 開發人員會感興趣的新功能和改良功能以及指導方針的集合。 新的命名空間新增至 Windows SDK 的完整清單，請參閱 < [Windows 10 組建 18362 API 變更](windows-10-build-18362-api-diff.md)。 如需 Windows 10 重點功能的詳細資訊，請參閱 [Windows 10 中有哪些酷功能](https://go.microsoft.com/fwlink/?LinkId=823181)。

## <a name="design--ui"></a>設計與 UI

功能 | 描述
:------ | :------
AnimatedVisualPlayer | [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer) API 裝載和控制動畫的視覺效果，您的應用程式中播放。 此 API 用來控制，並顯示內容，如[Lottie](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)視覺效果，可讓您呈現原生應用程式中的 Adobe AfterEffects 動畫。
CompactDensity | 啟用[精簡模式](../design/style/spacing.md)在您的應用程式可讓密集、 資訊豐富的控制項群組。 這可以協助進行瀏覽大量內容，最大化可見的內容，在頁面上，或當使用者正在使用指標輸入時協助巡覽和互動。
項目重複項 | [ItemsRepeater](../design/controls-and-patterns/items-repeater.md)控制項可以生物向使用者顯示集合的自訂體驗。 ItemsRepeater 不提供完整的使用者體驗或預設 UI。 相反地，它是您可用來建立您自己的唯一集合為基礎的體驗和自訂控制項的建置組塊。
教學提示 | A[教導提示](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)是半持續性和內容豐富飛出視窗上提供內容資訊。 您可以使用這個控制項的通知，提醒，並教導使用者有關新的或重要功能。
UI 命令 | 具有[UWP 應用程式的命令執行](../design/controls-and-patterns/commanding.md)，使用[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)並[StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)類別 （連同 ICommand 介面） 來共用及管理跨各個不同的命令控制項類型，不論所使用的裝置和輸入類型為何。
Windows UI 程式庫 | 最新官方版本 Windows 的 UI 程式庫 – [WinUI 2.1](https://docs.microsoft.com/uwp/toolkits/winui/release-notes/winui-2.1) – 活躍的新 XAML 控制項提供您的 Windows 應用程式。 WinUI 程式庫 API 可在較舊版本的 Windows 10 上執行，因此您並不需要加入版本檢查或條件式 XAML 來支援沒有使用最新作業系統的使用者。
視覺分層中的桌面應用程式 | 您現在可以[使用桌面應用程式中的 UWP 視覺層級 Api](../composition/visual-layer-in-desktop-apps.md)。 這些 Api 提供高效能模式重新定型 API 圖形、 效果和動畫，且是 UI 跨 Windows 裝置的基礎。
Z 深度和陰影 | 使用[Z 深度和陰影](../design/layout/depth-shadow.md)建立 UWP 應用程式中的 提高權限。 此新功能可讓您以您的應用程式 UI 比較容易進行掃描，並進一步傳達最重要的使用者將焦點放在。

## <a name="develop-windows-apps"></a>開發 Windows 應用程式

功能 | 描述
:------ | :------
反惡意程式碼掃描介面 (AMSI) | 了解[反惡意程式碼掃描介面 (AMSI) 如何協助您抵禦惡意程式碼](https://docs.microsoft.com/windows/desktop/amsi/how-amsi-helps)，然後看看[範例程式碼](https://docs.microsoft.com/windows/desktop/amsi/dev-audience)以了解如何在您的桌面應用程式中加以實作。
C++/WinRT 2.0 | 2.0 版的C++/WinRT 已被釋放。 請參閱[的新功能C++/WinRT](../cpp-and-winrt-apis/news.md)的完整 run-down 的所有新變更和新增項目。
選擇您的平台 | 想要建立新的桌面應用程式嗎？ 請查看我們改寫[選擇您的平台](https://docs.microsoft.com/windows/desktop/choose-your-technology)頁面的詳細的描述和 UWP、 WPF 和 Windows Form 的平台和 Win32 API 的進一步資訊的比較。
交談式代理程式 | [Windows.ApplicationModel.ConversationalAgent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent)命名空間可讓您新增至您的 Windows 應用程式的 Windows 平台代理程式啟用執行階段 (AAR) 所支援的任何數位協助。
雲端檔案服務 API | *雲端檔案 API*可讓您[建置雲端同步處理引擎支援預留位置檔案](https://docs.microsoft.com/windows/desktop/cfapi/build-a-cloud-file-sync-engine)。
直接 3D 12 | [Direct3D 12 轉譯階段](/windows/desktop/direct3d12/direct3d-12-render-passes)可以改善效能的您轉譯器為依據，並排式延後轉譯 (TBDR)，如果各種技巧。 此技術可協助您藉由啟用您的應用程式更輕鬆識別資源轉譯順序需求和資料相依性的 GPU 效率的轉譯器。 這會減少從晶片關閉記憶體的記憶體流量。
直接 Machine Learning (DirectML) | [DirectML](https://docs.microsoft.com/windows/desktop/direct3d12/dml)是機器學習服務的低階硬體加速 API。 它具有熟悉 (原生C++，nano COM) 程式設計介面和工作流程中的 DirectX 12 的樣式。 您可以將機器學習服務整合到您的遊戲、 引擎、 中介軟體、 後端或其他應用程式的推斷工作負載。 DirectML 受到所有 DirectX 12 相容的硬體。
DirectX HLSL | [HLSL 著色器模型 6.4](https://docs.microsoft.com/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12)搭配 DirectML 提供新機器學習服務內建函式。
驅動程式開發 | 新的音訊、 相機、 顯示、 網路、 行動式寬頻、 列印、 感應器、 儲存體及 wifi 的功能已新增適用於 Windows 的驅動程式開發人員。 請參閱[的新驅動程式開發功能](https://docs.microsoft.com/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest)如需詳細資訊。
檔案系統作業 | 這[最佳做法指南](../files/best-practices-for-writing-to-files.md)可協助您充分利用 Windows.Storage.FileIO 和 Windows.Storage.PathIO 類別來執行檔案系統 I/O 作業。
遊戲台與遙控器的互動 | 使用[遊戲台和遠端控制互動](../design/input/gamepad-and-remote-interactions.md)建置可用且可存取的互動體驗。 使用這些互動，您的應用程式可以為直覺式且容易使用的兩個英尺因為它是從十個英呎遠。
日文的紀元變更 | 我們提供了[這些指示](../design/globalizing/japanese-era-change.md)為各位示範如何確保您的 Windows 應用程式已準備好日文紀元變更集於 2019 5 月 1 日生效。 [此頁面也會適用於日文](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)。
WPF、 Windows Form 和 WinUI 的開放原始碼 | WPF、 Windows Form 和 WinUI UX 架構現在可供在 GitHub 上的開放原始碼投稿文章。 如需詳細資訊和連結，請參閱 <<c0> [ 建置 Windows 應用程式部落格](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)。
Xbox 的漸進式 Web 應用程式 | 具有[漸進式的 Web Apps for Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)，您可以擴充 web 應用程式，並使它成為 Xbox One 的應用程式透過 Microsoft Store 同時仍繼續使用您現有的架構、 CDN 及伺服器的後端。 大部分的情況下，您可以 Xbox one 封裝您 PWA，Windows 您平常的方式相同。 本指南將逐步引導您完成此程序，並反白顯示主要的差異。
Project Rome | Project Rome SDK 現在是適用於 Android 和 iOS。 了解如何整合圖形通知每個平台：[Android](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-android)並[iOS](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-ios)。
遠端的相機 | 使用 DeviceWatcher 類別，來[連接到遠端的相機](../audio-video-camera/connect-to-remote-cameras.md)，並讀取這些相機中的框架，併入 Windows 應用程式。
UWP 控制項，在傳統型應用程式 （XAML 群島） | Windows SDK，UWP 控制項裝載於 WPF、 Windows Form 中的 Api 和C++Win32 桌面應用程式將不再以開發人員預覽。 如需詳細資訊，請參閱 <<c0> [ 桌面應用程式中的 UWP 控制項](../xaml-platform/xaml-host-controls.md)。
Visual Studio 2019 | 已發行 visual Studio 2019，最新的工具和適用於任何開發人員、 應用程式或平台服務。 請參閱[的新功能 Visual Studio 2019](https://docs.microsoft.com/visualstudio/ide/whats-new-visual-studio-2019?view=vs-2019)若要了解最新版本，並開始。
Win32 web 檢視 | 我們[常見問題集](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)時使用 Microsoft Edge 網頁檢視，在桌面應用程式，以及範例和其他資源連結中提供常見問題的解答。
Windows 命令列 | [新的主控台功能](https://devblogs.microsoft.com/commandline/new-experimental-console-features/)包含實驗性的 「 終端機 索引標籤，以捲動、 指標形狀和資料指標色彩的設定。 深入了解[Windows 命令列工具適用於開發人員部落格](https://devblogs.microsoft.com/commandline/)。
Windows 社群工具組 | Windows 社群工具組 v5.1 動畫、 遠端裝置、 裁剪的影像和協助工具，提供令人興奮的更新。 </br> • 新[Lottie Windows 文件庫](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie)在 Windows 10 (1809) 上提供高品質動畫的支援，藉由使用 Windows.UI.Composition Api，並允許的耗用量[Bodymovin](https://aescripts.com/bodymovin/) JSON 檔案或最佳化程式碼產生的類別，在 Windows 應用程式中播放。 試用新[Lottie 檢視器應用程式](https://aka.ms/lottieviewer)從 Microsoft Store，若要測試動畫，並產生最佳化的程式碼，為您的 Windows 應用程式。 </br> • 新[遠端裝置選取器](https://docs.microsoft.com/windows/communitytoolkit/controls/remotedevicepicker)可讓使用者選取的裝置 (proximally 或可存取的雲端)、 啟動該裝置上的應用程式，或與遠端裝置上的應用程式服務通訊。 </br> • 新[ImageCropper 控制項](https://docs.microsoft.com/windows/communitytoolkit/controls/imagecropper)整合裁剪功能，來選取設定檔圖片，或使用相片編輯工具。 </br> • 此外，已有在控制項上的協助工具改善[Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0 預覽的 WPF 和 WinForms 和更多的功能，您可以深入了解中的封裝更新[版本資訊](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0).
Windows Machine Learning | 我們已重新設計 Windows AI 文件，將它們劃分成三個區域：Windows 機器學習服務 (WinML)、 Windows 願景技能，並直接的機器學習服務 (DirectML)。 查看新[登陸頁面](https://docs.microsoft.com/windows/ai/) </br> • [ *MLGen*體驗](https://docs.microsoft.com/windows/ai/mlgen)變更 Visual Studio 中。 於 Windows 10 版 1903年*mlgen*不再包含在 Windows 10 SDK。 如果您使用 VS 2017，則您應該改為下載並安裝 Visual Studio 擴充功能[Windows 機器學習服務程式碼產生器 VS 2017](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen)。 如果您使用 Visual Studio 2019，您應該安裝[Windows 機器學習服務程式碼產生器](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2)延伸模組。 </br> • 我們也很榮幸宣布新的支援封裝的權數。 開發人員現在可以降低其 ML 模型的磁碟使用量稱為權數封裝，可透過一種技術[WinMLTools 轉換器](https://docs.microsoft.com/windows/ai/convert-model-winmltools)。
合併的 WinRT 參考 | 我們已新增的完整說明[WinRT 型別系統](https://docs.microsoft.com/uwp/winrt-cref/winrt-type-system)並[WinMD 檔案](https://docs.microsoft.com/uwp/winrt-cref/winmd-files)，以提供特定注意事項深入解 WinRT Api 結構的定義。
適用於 Linux 的 Windows 子系統 (WSL) | [最新的更新 WSL](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/)包括能夠從 Windows 使用檔案總管 中和一些新的命令 wsl.exe 和 wslconfig.exe 存取 Linux 檔案。
Windows Vision Skills | [Windows 願景技能](https://docs.microsoft.com/windows/ai/windows-vision-skills)是一組 Api，可讓您建立 「 技能，「 例如臉部辨識，然後封裝它們以 NuGet 套件可使用的其他應用程式，甚至不需包括機器學習模型。

## <a name="publish--monetize-windows-apps"></a>發佈 Windows 應用程式以及從中獲利

功能 | 描述
 :------ | :------
MSIX | [在 Windows 10 上的 MSIX 支援建置 1709年和 1803年](https://docs.microsoft.com/windows/msix/msix-1709-and-1803-support)描述 MSIX 可支援的功能在 Windows 10 版本 1809年之前的版本。
MSIX 封裝和部署 | 我們引進了數個[修改封裝的相關改進](https://docs.microsoft.com/windows/msix/modification-package-insider-preview-build-18312)讓您更輕鬆地封裝自訂 MSIX 封裝中。 這些增強功能包括新**rescap6:ModificationPackage**封裝資訊清單、 覆寫修改封裝時，與主要套件中的之檔案的功能和套件檔案系統的功能中的項目架構外掛程式為 MSIX 修改封裝。
MSIX 封裝工具 | 我們已新增的 •[支援在遠端電腦上執行的轉換](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup)。 我們也引進了[MSIX 封裝工具測試人員計畫](https://docs.microsoft.com/windows/msix/packaging-tool/insider-program)能夠提早存取新工具功能。 </br> • [MSIX 封裝支援及更新版本 1709年上](https://docs.microsoft.com/windows/msix/packaging-tool/support-on-1709-and-later)提供專為 Windows 10 1709年和 1803年版建置套件使用 MSIX 封裝工具的相關指引。 </br> • [MSIX 封裝環境上 HYPER-V 快速建立](https://docs.microsoft.com/windows/msix/packaging-tool/quick-create-vm)示範如何建立虛擬環境 MSIX 封裝專案。 </br> •[配套 MSIX 封裝](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages)提供建立使用 MSIX 封裝工具的套件組合的指示。 </br> •[修改封裝，在 Windows 10 版本 1809年](https://docs.microsoft.com/windows/msix/modification-package-1809-update)包含使用 MSIX 封裝工具和 MakeApp.exe 1809 和更新版本的版本建立 Windows 10 版的修改套件的指示。
MSIX SDK | [使用 MSIX SDK 來建置跨平台使用的套件](https://docs.microsoft.com/windows/msix/msix-sdk/sdk-guidance)，並了解如何指定您要擷取封裝的目標平台。

## <a name="microsoft-learn"></a>Microsoft Learn

Microsoft 了解 Microsoft 開發人員，提供新的實際操作學習和訓練機會。

* 如果您有興趣了解如何開發 Windows 應用程式，請參閱[我們新的學習路徑](https://docs.microsoft.com/learn/paths/develop-windows10-apps/)的平台、 工具，以及如何撰寫您第一次的幾個應用程式的完整介紹。

* 想要了解如何將 UI 功能新增至您的 Windows 應用程式嗎？ 了解如何[建立 UI](https://docs.microsoft.com/learn/modules/create-ui-for-windows-10-apps/)，[新增至您的 UI 的瀏覽和媒體](https://docs.microsoft.com/learn/modules/enhance-ui-of-windows-10-app/)，或[實作資料繫結](https://docs.microsoft.com/learn/modules/implement-data-binding-in-windows-10-app/)。

* 如果您想要在 Web 開發過程中，請參閱[開發 web 應用程式，使用 Visual Studio Code](https://docs.microsoft.com/learn/modules/develop-web-apps-with-vs-code/)或是[建置簡單的網站](https://docs.microsoft.com/learn/modules/build-simple-website/)。

* 或者，任意瀏覽[Microsoft 了解上的所有 Windows 開發人員模組](https://docs.microsoft.com/learn/browse/?products=windows&resource_type=module)。

## <a name="videos"></a>影片

### <a name="progressive-web-apps"></a>漸進式 Web 應用程式

漸進式 Web 應用程式都是跨不同的瀏覽器和 Windows 10 裝置的各種不同的函式等原生應用程式的網站。 [觀看影片](https://youtu.be/ugAewC3308Y)若要進一步了解，然後[簽出文件](https://aka.ms/Windows-PWA)開始著手。

### <a name="vs-code-series"></a>VS 程式碼系列

請查看我們[新的影片系列 Visual Studio Code 上](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)for VSCode 的是什麼，如何使用它，以及建立方式的相關資訊。

### <a name="mixed-reality-services"></a>混合的實境服務

最近已宣布 HoloLens 2。 請參閱這[影片系列上混合實境](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST)的最新資訊，以及如何參與，並開始開發。

### <a name="one-dev-question"></a>一個開發人員問題

在一個開發人員問題影片系列中，資深的 Microsoft 開發人員會討論一系列有關 Windows 開發、 team 文化特性和歷程記錄的問題。

* [Raymond Chen 在 Windows 開發和歷程記錄](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman Windows 開發和歷程記錄](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafson 漸進式 Web 應用程式](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Chris Heilmann webhint 工具](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)
