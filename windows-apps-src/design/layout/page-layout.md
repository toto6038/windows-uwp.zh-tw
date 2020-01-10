---
title: UWP 應用程式的頁面配置
description: 設計您的應用程式時，首先要考慮的事項是版面配置結構。 本文涵蓋基本頁面配置的通用結構，包括您需要的 UI 元素，以及它們在頁面上的位置。 在 UWP 應用程式中，每個頁面通常都會有導覽、命令和內容元素。
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: 7333cebc945715412e3ff1140ca26e1ed5368704
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684541"
---
# <a name="page-layout"></a>頁面配置

在 UWP 應用程式中，每個[**頁面**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)通常都會有導覽、命令和內容元素。 

您的應用程式可以有多個頁面：當使用者啟動 UWP 應用程式時，應用程式代碼會建立一個[**框架**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)以放在應用程式的[**視窗**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window)內。 然後，框架可以在應用程式的[**頁面**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)實例之間[流覽](../basics/navigate-between-two-pages.md)。 

大部分的頁面都遵循通用的版面配置結構，本文涵蓋您需要的 UI 元素，以及它們在頁面上的位置。 

![頁面結構](images/page-components.svg)

## <a name="navigation"></a>瀏覽
您的應用程式佈建會從您選擇的導覽模型開始，這會定義您的使用者在應用程式頁面之間的流覽方式。 在本文中，我們將討論兩個常見的導覽模式： [左側流覽] 和 [頂端 nav]。 如需選擇其他導覽選項的指引，請參閱[UWP 應用程式的導覽設計基本概念](../basics/navigation-basics.md)。

![左上角導覽模式](images/top-left-nav.svg)

### <a name="left-nav"></a>左側導覽
左側導覽或流覽[窗格](../controls-and-patterns/navigationview.md)模式通常會保留給應用層級的流覽，並存在於應用程式中的最高層級，這表示它應該一律為可見且可供使用。 當您的應用程式中有超過五個流覽專案或超過五頁時，建議您向左導覽。 流覽窗格模式通常包含：
- 瀏覽項目
- 進入應用程式設定的進入點
- 進入帳戶設定的進入點

[NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)控制項會實作為 UWP 的左側導覽模式。

選取導覽專案時，畫面格應該會流覽至選取專案的頁面。

![流覽窗格已展開](images/navview-expanded.svg)

[功能表] 按鈕可讓使用者展開和折迭 [流覽] 窗格。 當螢幕大小大於 640 px 時，按一下 [功能表] 按鈕會將流覽窗格折迭為橫條。

![流覽窗格 compact](images/navview-compact.svg)

當螢幕大小小於 640 px 時，流覽窗格會完全折迭。

![流覽窗格最小](images/navview-minimal.svg)

### <a name="top-nav"></a>熱門導覽

Top nav 也可以做為最上層導覽。 雖然左側導覽可折迭，但最上層的導覽一律是可見的。 [NavigationView](../controls-and-patterns/navigationview.md)控制項會針對 UWP 執行頂端導覽和索引標籤模式。

![熱門導覽](images/pivot-large.svg)

## <a name="command-bar"></a>命令列

接下來，您可能會想要讓使用者輕鬆存取應用程式最常見的工作。 [命令列](../controls-and-patterns/app-bars.md)可提供應用層級或頁面層級命令的存取權，並可與任何導覽模式搭配使用。

![頂端的命令列位置 ](images/app-bar-desktop.svg)

命令列可以放在頁面的頂端或底部，視何者最適合您的應用程式。

![命令列放置於底部](images/app-bar-mobile.svg)

## <a name="content"></a>內容

最後，內容會因應用程式而有很大的差異，因此您可以用許多不同的方式呈現內容。 在此，我們會說明一些您可能想要在應用程式中使用的常見頁面模式。 許多應用程式會使用這些常見頁面模式的一部分或全部，來顯示不同類型的內容。 同樣地，您可以隨意混合和比對這些模式，以優化您的應用程式。

## <a name="landing"></a>登陸

![登陸頁面](images/hero-screen.svg)

登陸頁面，也就是主題畫面，通常出現在應用程式體驗的最上層。 大型介面區域做為應用程式的舞台，強調出使用者會想要瀏覽和使用的內容。

## <a name="collections"></a>集合

![圖庫](images/gridview.svg)

集合可讓使用者瀏覽內容或資料群組。 [方格檢視](../controls-and-patterns/item-templates-gridview.md)是相片或媒體為主內容的理想選擇，而[清單檢視](../controls-and-patterns/item-templates-listview.md)是文字為主內容或資料的理想選擇。

## <a name="masterdetail"></a>主要/詳細資料

![主要詳細資料](images/master-detail.svg)

[主要/詳細資料](../controls-and-patterns/master-details.md)模型是由清單檢視 (主要) 與內容檢視 (詳細資料) 所組成。 這兩個窗格都是固定的，並可垂直捲動。 選取清單視圖中的專案時，會對內容視圖進行相應的更新。 

## <a name="forms"></a>表單
![表單](images/form.svg)

[表單](../controls-and-patterns/forms.md)是一組控制項，會收集和提交使用者的資料。 大多數 (即使不是全部) 應用程式都會在設定頁面上、登入入口網站、意見反應中樞、帳戶建立或基於其他目的使用某種表單。 

## <a name="sample-apps"></a>範例應用程式
若要瞭解如何執行這些模式，請查看我們的[UWP 範例應用程式](https://developer.microsoft.com/windows/samples)：
- [BuildCast 影片播放機](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler (午餐排程器)](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book (著色本)](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [客戶訂購資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)