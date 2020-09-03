---
title: 2017 年 9 月 Windows 文件的新增功能
description: 新功能、影片及開發人員指引已加入 2017 年 9 月的 Windows 10 開發人員文件中
keywords: 新增功能, 更新, 功能, 開發人員指引, Windows 10, 1709
ms.date: 09/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 529dc61885f6bb0086432cd1d6209d9519a0114c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174332"
---
# <a name="whats-new-in-the-windows-developer-docs-in-september-2017"></a>2017 年 9 月 Windows 開發人員文件的新增功能

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 最近已有下列功能概觀、開發人員指引和範例可以使用，包含提供給 Windows 開發人員的全新及更新資訊。

當然，Fall Creators Update 即將推出，所以請拭目以待，下個月會有更多文件發行！

在 Windows 10 上[安裝工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank) 之後，就表示您已經準備好[建立新的通用 Windows 應用程式](../get-started/your-first-app.md)，或是探索[如何在 Windows 上使用現有的應用程式程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="xbox-live-creators-program"></a>Xbox Live 創作者計畫

Xbox Live 創作者計畫現在已上線，可讓您輕鬆地建立並發行能在 Windows 10 電腦和 Xbox One 主控台上執行的 UWP 遊戲。 如需詳細資訊，請參閱[開始使用 Xbox Live 創作者計畫](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)。

## <a name="developer-guidance"></a>開發人員指引

### <a name="xaml-basics-tutorials"></a>XAML 基本知識教學課程

我們已撰寫四份 [XAML 基本知識教學課程](../design/basics/xaml-basics-ui.md)並伴隨新的 [PhotoLab 範例](https://github.com/Microsoft/Windows-appsample-photo-lab)，涵蓋四個 XAML 程式設計的主要層面：使用者介面、資料繫結，自訂樣式和調適型配置。 每個教學課程科目都是從部分完成版本的 PhotoLab 範例開始，然後逐步建置應用程式缺少的一個元件。 

![顯示影像中心頁面的 PhotoLab 範例螢幕擷取畫面](images/PhotoLab-gallery-page.png)  

以下是新文章的快速概觀：

+ [**建立使用者介面**](../design/basics/xaml-basics-ui.md)示範如何建立基本影像中心介面。
+ [**建立資料繫結**](../data-binding/xaml-basics-data-binding.md)示範如何將資料繫結新增至影像中心，並以實際影像資料填入其中。
+ [**建立自訂樣式**](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-basics-style)示範如何將花俏的自訂樣式新增至相片編輯功能表。
+ [**建立調適型配置**](../design/basics/xaml-basics-adaptive-layout.md)示範如何讓圖庫版面配置變得可彈性調整，以便在所有裝置和螢幕大小上都看起來都很適當。

### <a name="get-started-tutorials"></a>入門教學課程

UWP 文件的「入門」章節已更新為使用[教學課程章節的全新登陸頁面](../get-started/create-uwp-apps.md)。 本章節提供入門體驗的全新和改進結構，協助使用者輕鬆地尋找並使用適用於他們的教學課程 - 包括上述的 XAML 基本知識教學課程。

### <a name="voice-and-tone"></a>語氣和語調

我們新增新的 [UWP 應用程式中的語氣和語調指引](../design/style/writing-style.md)，提供撰寫應用程式中文字的建議。 無論您要創造什麼，請切記您使用的語言必須平易近人、友善且具有資訊性。

## <a name="samples"></a>範例

### <a name="photolab-sample"></a>PhotoLab 範例

[PhotoLab 範例](https://github.com/Microsoft/windows-appsample-photo-lab)提供基本「影像中心」和編輯相片的體驗。

![顯示編輯相片頁面的 PhotoLab 範例螢幕擷取畫面](images/PhotoLab-editing-page.png)  

### <a name="customer-orders"></a>客戶訂單

[客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)範例已更新為使用新的 .NET Core 2.0 和 Entity Framework。