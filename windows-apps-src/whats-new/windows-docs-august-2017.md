---
title: 2017 年 8 月 Windows 文件的最新動向 - 開發 UWP app
description: 新功能、影片及開發人員指引已加入 2017 年 8 月的 Windows 10 開發人員文件中
keywords: 最新動向, 更新, 功能, 開發人員指引, Windows 10, 1708
ms.date: 08/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: aeb6f60396b270a78df5203106635436fe2dabe5
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922841"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2017"></a>2017 年 8 月 Windows 開發人員文件的最新動向

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 最近已有下列功能概觀、開發人員指引和影片可以使用，包含提供給 Windows 開發人員的全新及更新資訊。

在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/your-first-app.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="windows-template-studio"></a>Windows Template Studio

使用新的適用於 Visual Studio 2017 的 [Windows Template Studio](https://aka.ms/wtsinstall) 擴充功能，快速建置包含您所需頁面、架構及功能的 UWP app。 這個以精靈為主的使用體驗實作經證實有效的模式及最佳做法，可節省新增功能至應用程式的時間並且避開相關問題。

![Windows Template Studio](images/template-studio.png)

### <a name="conditional-xaml"></a>條件式 XAML

您現在可以預覽[條件式 XAML](../debug-test-perf/conditional-xaml.md) 來建立[版本調適型應用程式](../debug-test-perf/version-adaptive-apps.md)。 條件式 XAML 可讓您在 XAML 標記中使用 ApiInformation.IsApiContractPresent method，因此可以根據 API 是否存在來設定屬性和具現化物件，而不必使用程式碼後置。

### <a name="game-mode"></a>遊戲模式

通用 Windows 平台 (UWP) 的[遊戲模式](https://msdn.microsoft.com/library/windows/desktop/mt808808) API 可讓您利用 Windows 10 中的遊戲模式產生最佳化的遊戲體驗。 這些 API 位於 **&lt;expandedresources.h&gt;** 標頭檔中。

![遊戲模式](images/game-mode.png)

### <a name="submission-api-supports-video-trailers-and-gaming-options"></a>提交 API 支援預告片和遊戲選項

[Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 現在可讓您使用 App 提交來包含[預告片](../monetize/manage-app-submissions.md#trailer-object)和[遊戲選項](../monetize/manage-app-submissions.md#gaming-options-object)。


## <a name="developer-guidance"></a>開發人員指引

### <a name="data-schemas-for-store-products"></a>市集產品的資料結構描述

我們新增了[市集產品的資料結構描述](../monetize/data-schemas-for-store-products.md)文章。 此文章針對 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中的幾個物件 (包括 [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) 和 [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense)) 提供其市集相關資料的結構描述。

### <a name="desktop-bridge"></a>傳統型橋接器

我們新增了兩份指南，可協助您加入讓 Windows 10 使用者耳目一新的現代化體驗。

請參閱[增強您的 Windows 10 傳統型應用程式](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance)以尋找和參考正確的檔案，然後撰寫程式碼來增強 UWP 體驗，讓 Windows 10 使用者眼睛為之一亮。  

請參閱[使用現代化 UWP 元件擴充您的傳統型應用程式](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend)，以結合現代化 XAML UI 與其他必須在 UWP app 容器中執行的 UWP 體驗。

### <a name="getting-started-with-point-of-service"></a>開始使用服務點

我們加入了新的指南，可協助您[開始使用點服務裝置](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-get-started)。 內容涵蓋裝置列舉、檢查裝置功能、宣告裝置和裝置共用等主題。 

### <a name="xbox-live"></a>Xbox Live

我們已經為 Xbox Live 開發人員新增關於 UWP 遊戲和 Xbox 開發人員套件 (XDK) 遊戲的文件。

請參閱 [Xbox Live 開發人員指南](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/)，以了解如何使用 Xbox Live API 將您的遊戲連線至 Xbox Live 社交遊戲網路。

任何 UWP 遊戲開發人員都可以利用 [Xbox Live 創作者計畫](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators)，在電腦和 Xbox One 上開發並發行支援 Xbox Live 的遊戲。

請參閱 [Xbox Live 開發人員計畫概觀](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview)，以取得提供給 Xbox Live 開發人員的程式和功能的相關資訊。

## <a name="videos"></a>影片

### <a name="mixed-reality"></a>混合實境

[Microsoft HoloLens 課程 250](https://developer.microsoft.com/en-us/windows/mixed-reality/mixed_reality_250) 已發行一系列新的教學課程影片。 如果您已安裝工具且熟悉開發混合實境的基本知識，請觀看這些有關在各種混合實境裝置上建立共用體驗的視訊課程。

### <a name="narrator-and-dev-mode"></a>朗讀程式和開發人員模式

您可能已經知道您可以使用[[朗讀程式]](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)來測試應用程式的螢幕助讀使用體驗。 不過，朗讀程式還有開發人員模式，為您提供很好的視覺化呈現方式來展示顯露給朗讀程式的資訊。 [觀看影片](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode)，然後深入了解[朗讀程式開發人員模式](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode)。

### <a name="windows-template-studio"></a>Windows Template Studio

[這個影片](https://channel9.msdn.com/Blogs/One-Dev-Minute/Getting-Started-with-Windows-Template-Studio)提供了更詳細的 Windows Template Studio 概觀。 當您準備好時，請[安裝擴充功能](https://aka.ms/wtsinstall)或[查看原始程式碼和文件](https://aka.ms/wtsinstall)。