---
author: QuinnRadich
title: 2017 年 12 月 Windows 文件的最新動向 - 開發 UWP app
description: 新功能、影片及開發人員指引已加入 2017 年 12 月的 Windows 10 開發人員文件中
keywords: 最新動向, 更新, 功能, 開發人員指引, Windows 10, 12 月
ms.author: quradic
ms.date: 12/14/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 93ef3de0dc86ab9708f7be99836204c2232dfef4
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2018
ms.locfileid: "4129627"
---
# <a name="whats-new-in-the-windows-developer-docs-in-december-2017"></a>2017 年 12 月 Windows 開發人員文件的最新動向

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 發行 Fall Creators Update 後已有下列功能概觀、開發人員指引和範例可以使用，包含提供給 Windows 開發人員的全新及更新資訊。

在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="windows-mixed-reality-enthusiasts-guide"></a>Windows Mixed Reality：愛好者指南 (英文)

以深入混合實境世界的高科技愛好者為目標對象，[愛好者指南](https://docs.microsoft.com/en-us/windows/mixed-reality/enthusiast-guide/)回答大家對於 Windows Mixed Reality Windows 最常問的問題。 

本指南提供下列內容： 
- 購買之前常見問題集 
- 如何檢查您的電腦相容性 
- 安裝指示 
- 如何使用您的頭戴式裝置和控制器 
- 如何下載和播放身臨其境遊戲、360 影片、2D 應用程式、WebVR 以及 SteamVR 
- 如何疑難排解問題，及其他。

![Windows Mixed Reality 頭戴式裝置使用者及運動控制器](images/BeforeYouBegin-tile.jpg)

### <a name="keyboard-interactions"></a>鍵盤互動

設計和最佳化您的 UWP app，透過更新的[鍵盤互動](../design/input/keyboard-interactions.md)提供可存取體驗和功能給進階使用者。 我們已更新我們的建議和指導方針，以反映 Fall Creators Update 中新增的全新互動改良功能。

請參閱[鍵盤快速鍵](../design/input/keyboard-accelerators.md)和[自訂鍵盤互動](../design/input/custom-keyboard-interactions.md)以擴充您應用程式的鍵盤功能。

在支援觸控互動的裝置上，運用[回應觸控式鍵盤的出現](../design/input/respond-to-the-presence-of-the-touch-keyboard.md)和[使用輸入範圍來變更觸控式鍵盤](../design/input/use-input-scope-to-change-the-touch-keyboard.md)文章來新增鍵盤功能。

### <a name="microsoft-collaborate"></a>Microsoft Collaborate

Microsoft Collaborate 入口網站提供工具和服務，透過啟用工程系統工作項目 (錯誤、功能要求等) 的共用以及內容發佈 (組建、文件、規格) 來簡化 Microsoft 生態系統內的工程共同作業。 [進一步瞭解](https://docs.microsoft.com/en-us/collaborate)。

![開發人員中心儀表板中的 Microsoft Collaborate](images/microsoft_collaborate_screenshot.PNG)

### <a name="package-desktop-applications-with-uwp-projects"></a>將傳統型應用程式與 UWP 專案封裝在一起

Visual Studio 2017 版本 15.5 已經更新 **Windows 應用程式封裝專案**範本，現在納入 UWP 專案更簡單。 您不再需要使用 JavaScript 型封裝專案，接著還要手動調整套件資訊清單。  

如需如何使用這個新範本來封裝傳統型應用程式的指導方針，請參閱[使用 Visual Studio 封裝應用程式](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-packaging-dot-net)。

如需如何將 UWP 專案加入套件的指導方針，請參閱[使用現代化 UWP 元件擴充您的傳統型應用程式](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend)。

### <a name="subscription-add-ons-are-now-available-to-developers-in-the-windows-dev-center-insider-program"></a>訂閱附加元件現已在 Windows 開發人員中心測試人員計畫開放給測試人員取得

已加入[開發人員中心測試人員計畫](../publish/dev-center-insider-program.md)的所有開發人員，現在都可以使用訂閱附加元件，在其應用程式中運用自動週期性計費期間來銷售數位產品 (例如應用程式功能或數位內容)。 如需詳細資訊，請參閱[啟用應用程式的訂閱附加元件](../monetize/enable-subscription-add-ons-for-your-app.md)。

## <a name="developer-guidance"></a>開發人員指引

### <a name="color"></a>色彩

我們已加入一些新指導方針，說明如何在您的應用程式中使用色彩來提供最佳的使用者體驗。 這包括 API 使用案例，以及 UI 設計與存取性的一般指導方針。 我們也已更新 Xbox 上可用的使用者輔色清單。 [請至此處查看更新的色彩文章。](../design/style/color.md)

![通用 windows 調色盤](../design/basics/images/colors.png)

### <a name="data-access-guides"></a>資料存取指南

我們已新增 [SQL Server 指南](../data-access/sql-server-databases.md)來說明您的應用程式如何直接存取 SQL Server 資料庫。 不需要服務層級。

此外，我們已完全改造 [SQLite 指南](../data-access/sqlite-databases.md)，加上更俐落的外觀和感覺，我們也加入在使用者裝置上於輕量資料庫中儲存和擷取資料的最佳做法。

### <a name="forms"></a>表單

我們已經新增[如何在您的應用程式中建立表單](../design/controls-and-patterns/forms.md)一文，用以收集及提交使用者資料。 這包括實作表單的特定資訊以及使用表單之時機和位置的一般指導方針。

### <a name="intro-to-app-design"></a>應用程式設計簡介

通用 Windows 平台 (UWP) 設計指導方針可以協助您設計和建立美觀、優雅的應用程式。 [我們的新簡介](../design/basics/design-and-ui-intro.md)提供每個 UWP app 中包含的通用設計功能概觀，以及如何利用文章來建立可在各種裝置上精美縮放的使用者介面 (UI)。


### <a name="request-ratings-and-reviews"></a>請求評分與評論

我們已新增新文章，說明如何[為您的應用程式請求評分與評論](../monetize/request-ratings-and-reviews.md)。 您可以在應用程式內容中顯示評分與評論對話方塊，您也可以在 Store 中開啟應用程式的評分與評論頁面。

## <a name="samples"></a>範例

### <a name="customer-orders"></a>客戶訂單

[客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)範例已更新，現在會顯示資料存取 (例如使用存放庫模式) 的相關最佳做法，以及如何連接至多個資料來源 (包括 Sqlite、SQL Azure 及 REST 服務)。

## <a name="videos"></a>影片

### <a name="package-a-net-app-in-visual-studio"></a>在 Visual Studio 中封裝 .NET 應用程式

比以往更輕鬆地將您的傳統型應用程式攜入通用 Windows 平台。 [觀賞影片](https://www.youtube.com/watch?v=fJkbYPyd08w)以了解如何封裝您的 .NET 應用程式以供發佈，接著[查看這個頁面](../porting/desktop-to-uwp-packaging-dot-net.md)以取得詳細資訊。