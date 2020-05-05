---
title: 2019 年 1 月 Windows 文件的新增功能 - 開發 UWP 應用程式
description: 新功能、影片及開發人員指引已加入 2019 年 1 月的 Windows 10 開發人員文件中
keywords: 新增功能, 更新, 功能, 開發人員指引, Windows 10, 1 月
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7947fb6e71a9f2ddbedcd8e3ee8bab7b720dc444
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "74902464"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>2019 年 1 月 Windows 開發人員文件的新增功能

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能概觀、開發人員指引和影片已在 1 月份提供使用。

在 Windows 10 上[安裝工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank) 之後，就表示您已經準備好[建立新的通用 Windows 應用程式](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的應用程式程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="windows-development-on-microsoft-learn"></a>Microsoft Learn 上的 Windows 開發

Microsoft Learn 為 Microsoft 開發人員提供新的實際操作學習和訓練機會。 如果您有興趣了解如何開發 Windows 應用程式，請參閱[我們新的學習路徑](/learn/paths/develop-windows10-apps/)，以取得平台、工具，以及如何撰寫前幾個應用程式的完整介紹。

![Windows 開發學習路徑的影像](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

[Direct3D 12 轉譯行程](/windows/desktop/direct3d12/direct3d-12-render-passes)可以改善您的轉譯器效能 (如果它是以並排式延遲轉譯 (TBDR) 為基礎的話)。 此技術可讓您的應用程式更容易識別資源轉譯順序需求和資料相依性，因而減少往返晶片記憶體的記憶體流量，進而協助您的轉譯器改善 GPU 效率。

### <a name="msix-modification-packages"></a>MSIX 修改套件

Windows 10 版本 1809 改善了 [MSIX 修改套件](/windows/msix/modification-package-1809-update)的支援。 修改套件可以包含登錄型外掛程式和相關聯的自訂，且可讓透過 MSIX 部署的應用程式使用虛擬登錄並如預期般執行。

![MSIX 修改套件建立](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>WPF、Windows Forms 和 WinUI 的開放原始碼

WPF、Windows Forms 和 WinUI UX 架構現在可用於在 GitHub 上貢獻開放原始碼。 如需詳細資訊和連結，請參閱[建置 Windows 應用程式部落格](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)。

### <a name="progressive-web-apps-for-xbox"></a>適用於 Xbox 的漸進式 Web 應用程式

使用[適用於 Xbox One 的漸進式 Web 應用程式](/microsoft-edge/progressive-web-apps/xbox-considerations)，您可以擴充 Web 應用程式並讓它以 Xbox One 應用程式的形式透過 Microsoft Store 提供，同時仍繼續使用您現有的架構、CDN 及伺服器後端。 在大部分的情況下，您可以如同在 Windows 中一樣封裝 PWA for Xbox One，不過，本指南將引導您認識數個主要差異。

### <a name="windows-machine-learning"></a>Windows Machine Learning

我們已重新建構 [WinML API 的登陸頁面](/windows/ai/api-reference)，並新增 WinML 自訂運算子和原生 API 的新文件。

[使用 PyTorch 訓練模型](/windows/ai/train-model-pytorch)提供有關如何使用 PyTorch 架構在本機或雲端訓練模型的指引。 您可以接著將此模型下載為 ONNX 檔案，並在 WinML 應用程式中使用它。

![WinML 圖形](images/winml-graphic.png)

## <a name="developer-guidance"></a>開發人員指引

### <a name="choose-your-platform"></a>選擇您的平台

想要建立新的傳統型應用程式？ 請查看我們改造的[選擇您的平台](/windows/desktop/choose-your-technology)頁面，以取得 UWP、WPF 和 Windows Forms 平台的詳細描述和比較，以及 Win32 API 的進一步資訊。

### <a name="faqs-on-win32-webview"></a>Win32 WebView 的常見問題集

我們的[常見問題集](/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)會提供在傳統型應用程式中使用 Microsoft Edge WebView 時的常見問題解答，以及連範例和其他資源的連結。

### <a name="japanese-era-change"></a>日本年號變更

[針對日本年號變更準備您的應用程式](../design/globalizing/japanese-era-change.md)說明如何確保您的 Windows 應用程式已準備好因應 2019 年 5 月 1 日起生效的日本年號變更。 [此頁面也會以日文提供](/windows/uwp/design/globalizing/japanese-era-change)。

## <a name="videos"></a>視訊

### <a name="progressive-web-apps"></a>漸進式 Web 應用程式

漸進式 Web 應用程式是一個網站，其作用如同橫跨不同瀏覽器和各種 Windows 10 裝置的原生應用程式。 [觀看影片](https://youtu.be/ugAewC3308Y)進一步了解，然後[查看文件](https://developer.microsoft.com/windows/pwa)以開始使用。

### <a name="vs-code-series"></a>VS Code 系列

請查看 [Visual Studio Code 上新的影片系列](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)，以取得何謂 VSCode、其使用方式，以及其建立方式的相關資訊。

### <a name="one-dev-question"></a>One Dev Question

在 One Dev Question 影片系列中，資深的 Microsoft 開發人員會談論一系列關於 Windows 開發、團隊文化和歷史的問題。 以下是我們所回答的最新問題！

Raymond Chen：

* [為何有 Program Files 與 Program Files (x86)？](https://youtu.be/qRb6otsHG5c) (英文)
* [您第一次訪問 Microsoft 的情況為何？](https://youtu.be/MfzzbNp8kfw)

Larry Osterman：

* [COM 為何如此複雜？](https://youtu.be/-gkXAV-StVA) (英文)
* [您第一次訪問 Microsoft 的情況為何？](https://youtu.be/N7o9eJpFYco) (英文)