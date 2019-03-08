---
title: 在 Windows 文件中 2019 年 1 月新消息-開發 UWP 應用程式
description: 2019 年 1 月的 Windows 10 開發人員文件已加入新功能、 影片和開發人員指引
keywords: 新功能、 更新、 功能、 開發人員指導方針，Windows 10 年 1 月
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636573"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Windows 開發人員文件在 2019 年 1 月新消息

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列的功能概觀、 開發人員指引和影片已有可用在 1 月的。

在 Windows 10 上[安裝工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows app](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="windows-development-on-microsoft-learn"></a>Microsoft 了解 Windows 開發

Microsoft 了解 Microsoft 開發人員，提供新的實際操作學習和訓練機會。 如果您有興趣了解如何開發 Windows 應用程式，請參閱[我們新的學習路徑](https://docs.microsoft.com/learn/paths/develop-windows10-apps/)的平台、 工具，以及如何撰寫您第一次的幾個應用程式的完整介紹。

![映像的 Windows 開發學習路徑](images/windows-learn.png)

### <a name="direct-3d-12"></a>直接 3D 12

[Direct3D 12 轉譯階段](/windows/desktop/direct3d12/direct3d-12-render-passes)可以改善效能的您轉譯器為依據，並排式延後轉譯 (TBDR)，如果各種技巧。 此技術可協助您的轉譯器，並以改善 GPU 效率啟用您的應用程式更輕鬆識別資源轉譯順序需求和資料相依性，因此可減少從晶片關閉記憶體的記憶體流量。

### <a name="msix-modification-packages"></a>MSIX 修改套件

Windows 10 版本 1809年改進的支援[MSIX 修改封裝](https://docs.microsoft.com/windows/msix/modification-package-1809-update)。 修改套件可以包含登錄的外掛程式和相關聯的自訂，而且會啟用透過使用虛擬登錄，並如預期般執行 MSIX 部署的應用程式。

![MSIX 修改封裝建立](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>WPF、 Windows Form 和 WinUI 的開放原始碼

WPF、 Windows Form 和 WinUI UX 架構現在可供在 GitHub 上的開放原始碼投稿文章。 如需詳細資訊和連結，請參閱 <<c0> [ 建置 Windows 應用程式部落格](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)。

### <a name="progressive-web-apps-for-xbox"></a>Xbox 的漸進式 Web 應用程式

具有[漸進式的 Web Apps for Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)，您可以擴充 web 應用程式，並使它成為 Xbox One 的應用程式透過 Microsoft Store 同時仍繼續使用您現有的架構、 CDN 及伺服器的後端。 大部分的情況下，封裝您 PWA Xbox one Windows 您平常的方式相同，不過，有數個主要的差異，本指南將引導您完成。

### <a name="windows-machine-learning"></a>Windows 機器學習服務

我們已重新編排[WinML Api 的登陸頁面](https://docs.microsoft.com/windows/ai/api-reference)，並新增 WinML 自訂運算子和原生 Api 的新文件。

[訓練模型與 PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch)提供有關如何使用 PyTorch framework，在本機或雲端中定型模型的指引。 然後，您可以下載此模型為 ONNX 檔案，並 WinML 應用程式中使用它。

![WinML 圖形](images/winml-graphic.png)

## <a name="developer-guidance"></a>開發人員指引

### <a name="choose-your-platform"></a>選擇您的平台

想要建立新的桌面應用程式嗎？ 請查看我們改寫[選擇您的平台](https://docs.microsoft.com/windows/desktop/choose-your-technology)頁面的詳細的描述和 UWP、 WPF 和 Windows Form 的平台和 Win32 API 的進一步資訊的比較。

### <a name="faqs-on-win32-webview"></a>在 Win32 WebView 的常見問題集

我們[常見問題集](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)桌面的應用程式中使用 Microsoft Edge 網頁檢視時，提供問題的解答，以及連結至範例和其他資源。

### <a name="japanese-era-change"></a>日文的紀元變更

[準備您的應用程式，以日文的紀元變更](../design/globalizing/japanese-era-change.md)示範您如何確保您的 Windows 應用程式已準備日文紀元變更集，才會將放在 2019 5 月 1 日。 [此頁面也會適用於日文](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)。

## <a name="videos"></a>影片

### <a name="progressive-web-apps"></a>漸進式 Web 應用程式

漸進式 Web 應用程式都是跨不同的瀏覽器和 Windows 10 裝置的各種不同的函式等原生應用程式的網站。 [觀看影片](https://youtu.be/ugAewC3308Y)若要進一步了解，然後[簽出文件](https://aka.ms/Windows-PWA)開始著手。

### <a name="vs-code-series"></a>VS 程式碼系列

請查看我們[新的影片系列 Visual Studio Code 上](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)for VSCode 的是什麼，如何使用它，以及建立方式的相關資訊。

### <a name="one-dev-question"></a>一個開發人員問題

在一個開發人員問題影片系列中，資深的 Microsoft 開發人員會討論一系列有關 Windows 開發、 team 文化特性和歷程記錄的問題。 以下是我們所回答的最新問題 ！

Raymond Chen:

* [為什麼有 Program Files 與 Program Files (x86)？](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [COM，為何如此複雜嗎？](https://youtu.be/-gkXAV-StVA )
* [例如您第一次訪談 microsoft 是什麼？](https://youtu.be/qRb6otsHG5c)
