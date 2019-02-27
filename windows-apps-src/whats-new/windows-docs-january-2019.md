---
title: 在 2019 年 1 月 Windows 文件中新動向-開發 UWP app
description: 新功能、 影片及開發人員指引已新增 2019 年 1 月的 Windows 10 開發人員文件
keywords: 新動向，更新，功能，開發人員指引，Windows 10 年 1 月
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116250"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>在 2019 年 1 月中 Windows 開發人員文件的最新動向

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能概觀、 開發人員指引和影片已可供在 1 月的。

在 Windows10 上[安裝工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="windows-development-on-microsoft-learn"></a>Microsoft 了解在 Windows 開發

Microsoft 了解 Microsoft 開發人員提供新的實機操作學習和訓練的機會。 如果您感興趣學習如何開發 Windows 應用程式，請查看[我們新的學習路徑](https://docs.microsoft.com/learn/paths/develop-windows10-apps/)的平台，工具，以及如何撰寫您第一次的幾個應用程式的完整簡介。

![Windows 開發學習路徑的影像](images/windows-learn.png)

### <a name="direct-3d-12"></a>直接存取的 3D 12

如果它根據磚為基礎的延期轉譯 (TBDR)，在其他技術之間， [Direct3D 12 轉譯階段](/windows/desktop/direct3d12/direct3d-12-render-passes)可以改善您轉譯器的效能。 技術可協助您轉譯器移植到透過啟用更清楚地識別資源轉譯排序需求和資料的相依性，您的應用程式，並以減少記憶體流量從關閉晶片記憶體改善 GPU 效率。

### <a name="msix-modification-packages"></a>MSIX 修改套件

[MSIX 修改套件](https://docs.microsoft.com/windows/msix/modification-package-1809-update)的 Windows 10 版本 1809年改進支援。 修改套件可以包含以登錄為基礎的外掛程式和相關聯的自訂項目，並將會啟用透過使用的虛擬登錄並執行如預期般 MSIX 部署的應用程式。

![MSIX 修改套件建立](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>WPF、 Windows Form 及 WinUI 的開放原始碼

WPF、 Windows Form 及 WinUI UX 架構現在可供在 GitHub 上的開放原始碼貢獻。 如需詳細資訊和連結，請參閱[建置 Windows 應用程式部落格](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)。

### <a name="progressive-web-apps-for-xbox"></a>適用於 Xbox 的漸進式 Web 應用程式

使用[適用於 Xbox One 的漸進式 Web 應用程式](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)，您可以擴充 web 應用程式，並使其可做為 Xbox One 的應用程式透過 Microsoft 網上商店時仍繼續使用您現有的架構、 CDN 和伺服器的後端。 大多數的情況下，封裝您 PWA 針對 Xbox One 適用於 Windows 相同的方式，不過，有幾個重要的差異，本指南將逐步引導您完成。

### <a name="windows-machine-learning"></a>Windows 機器學習

我們已經重建[WinML Api 的登陸頁面](https://docs.microsoft.com/windows/ai/api-reference)，並新增新的文件 WinML 自訂電信業者與原生 Api。

[訓練模型與 PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch)提供如何訓練模型使用 PyTorch 架構在本機或在雲端中的指導方針。 然後，您可以下載此模型中的為 ONNX 檔案，並在 WinML 應用程式中使用它。

![WinML 圖形](images/winml-graphic.png)

## <a name="developer-guidance"></a>開發人員指引

### <a name="choose-your-platform"></a>選擇您平台

有興趣建立新的傳統型應用程式？ 請查看我們改頭換面[選擇您平台](https://docs.microsoft.com/windows/desktop/choose-your-technology)頁面以取得詳細的描述和 UWP、 WPF 及 Windows Forms 平台，並進一步 Win32 API 的詳細資訊的比較。

### <a name="faqs-on-win32-webview"></a>Win32 WebView 上的常見問題集

使用 Microsoft Edge WebView 中傳統型應用程式，以及範例和其他資源的連結時，我們的[常見問題集](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)提供常見問題的解答。

### <a name="japanese-era-change"></a>紀元變更

[準備您的應用程式針對日本紀元變更](../design/globalizing/japanese-era-change.md)，示範您如何請確定您的 Windows 應用程式已準備日本紀元變更設定為 \ [採取放在 2019 年 1 日。 [此頁面是為日文也可以使用](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)。

## <a name="videos"></a>影片

### <a name="progressive-web-apps"></a>漸進式 Web 應用程式

漸進式 Web 應用程式不會函式像原生應用程式，跨不同的瀏覽器和各種不同的 Windows 10 裝置的網站。 [觀看影片](https://youtu.be/ugAewC3308Y)以了解更多]，以及然後[查看文件](https://aka.ms/Windows-PWA)開始。

### <a name="vs-code-series"></a>VS 程式碼系列

請查看我們[新的影片系列，在 Visual Studio Code 上](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)適用於 VSCode 的功能、 如何使用它，並建立方式的相關資訊。

### <a name="one-dev-question"></a>一個開發人員問題

在開發人員問題影片系列，longtime Microsoft 開發人員會討論一系列的 Windows 開發、 小組文化特性和歷程記錄的相關問題。 以下是我們已經回答的最新問題 ！

Raymond Chen:

* [為什麼必須 Program Files 和 Program Files (x86)？](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [為何 COM 如此複雜嗎？](https://youtu.be/-gkXAV-StVA )
* [什麼是 microsoft 像您第一次訪談？](https://youtu.be/qRb6otsHG5c)
