---
title: UWP 應用程式的頁面配置
description: 在設計您的應用程式時，首先来考慮是配置結構。 本文章涵蓋基本版面配置，包括哪些 UI 項目，您將需要以及它們應在頁面上的一般結構。 在 UWP 應用程式，每個頁面通常會有導覽、 命令和內容項目。
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP
localizationpriority: medium
ms.openlocfilehash: edda9948e70b1757ddb46fae45a579bb2fdb8de1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633713"
---
# <a name="page-layout"></a>頁面配置

在 UWP 應用程式，每個[**頁**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)通常具有 瀏覽、 命令和內容項目。 

您的應用程式可以有多個頁面： 當使用者啟動的 UWP 應用程式，應用程式程式碼會建立[**框架**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)放置在應用程式內部[**視窗**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window). 接著可以在框架[瀏覽](../basics/navigate-between-two-pages.md)之間的應用程式[**頁面**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)執行個體。 

大部分頁面都遵循常見的版面配置結構，以及本文章涵蓋的 UI 項目您將需要以及它們應在頁面上。 

![頁面結構](images/page-components.svg)

## <a name="navigation"></a>瀏覽
您的應用程式版面配置開始瀏覽模型選擇時，會定義您的使用者在您的應用程式中的頁面之間巡覽的方式。 在本文中，我們將討論兩個常見的瀏覽模式： 左瀏覽和最上層導覽 如需選擇其他的巡覽選項的指引，請參閱 < [UWP 應用程式的瀏覽設計基本概念](../basics/navigation-basics.md)。

![框線和左瀏覽模式](images/top-left-nav.svg)

### <a name="left-nav"></a>左側瀏覽
左導覽列，或[瀏覽窗格](../controls-and-patterns/navigationview.md)模式，是通常保留給應用程式層級瀏覽最高層級的應用程式，這表示它應該永遠可見和可內存在。 有五個以上的導覽項目或您的應用程式中的五個頁面時，我們會建議左側瀏覽。 瀏覽 窗格模式通常會包含：
- 瀏覽項目
- 應用程式設定進入點
- 帳戶設定進入點

[NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)控制項會實作適用於 UWP 的左側瀏覽模式。

選取 瀏覽項目時，畫面格應該瀏覽至選取的項目頁面。

![展開的瀏覽窗格](images/navview-expanded.svg)

[功能表] 按鈕可讓使用者展開和摺疊瀏覽窗格。 當畫面大小大於 640 像素，按一下 [功能表] 按鈕的列會摺疊瀏覽窗格。

![精簡瀏覽窗格](images/navview-compact.svg)

當畫面大小小於 640 像素，瀏覽窗格完全摺疊。

![瀏覽窗格最小](images/navview-minimal.svg)

### <a name="top-nav"></a>瀏覽

瀏覽，也可以做為最上層導覽。 可摺疊左側瀏覽時，上面瀏覽永遠是可見的。 [NavigationView](../controls-and-patterns/navigationview.md)控制項會實作適用於 UWP 的最上層導覽及索引標籤模式。

![上方導覽](images/pivot-large.svg)

## <a name="command-bar"></a>命令列

接下來，您可能想為使用者提供輕鬆存取您的應用程式最常見工作。 A[命令列](../controls-and-patterns/app-bars.md)可以提供存取應用程式層級或頁面層級命令，以及可以搭配任何瀏覽模式。

![在頂端的命令列位置 ](images/app-bar-desktop.svg)

命令列可以放在頂端或底部的頁面上，較適合您的應用程式。

![在底部的命令列位置](images/app-bar-mobile.svg)

## <a name="content"></a>內容

最後，內容而不同，廣泛的應用程式即可在應用程式，您可以在許多不同的方式來呈現內容。 在這裡，我們會說明一些常見的頁面模式可能會想要使用您的應用程式中。 許多 App 會使用這些常見頁面模式的一部分或全部，來顯示不同類型的內容。 同樣地，隨意混合及比對這些模式，以最佳化您的應用程式。

## <a name="landing"></a>登陸

![登陸頁面](images/hero-screen.svg)

登陸頁面，也就是主題畫面，通常出現在 App 體驗的最上層。 大型介面區域做為 App 的舞台，強調出使用者會想要瀏覽和使用的內容。

## <a name="collections"></a>集合

![圖庫](images/gridview.svg)

集合可讓使用者瀏覽內容或資料群組。 [方格檢視](../controls-and-patterns/item-templates-gridview.md)是相片或媒體為主內容的理想選擇，而[清單檢視](../controls-and-patterns/item-templates-listview.md)是文字為主內容或資料的理想選擇。

## <a name="masterdetail"></a>主要/詳細資料

![主要詳細資料](images/master-detail.svg)

[主要/詳細資料](../controls-and-patterns/master-details.md)模型是由清單檢視 (主要) 與內容檢視 (詳細資料) 所組成。 這兩個窗格都是固定的，並可垂直捲動。 選取清單檢視中的項目時，也會跟著更新內容的檢視。 

## <a name="forms"></a>表單
![表單](images/form.svg)

[表單](../controls-and-patterns/forms.md)是一組控制項，會收集和提交使用者的資料。 大多數 (即使不是全部) App 都會在設定頁面上、登入入口網站、意見反應中樞、帳戶建立或基於其他目的使用某種表單。 

## <a name="sample-apps"></a>範例應用程式
若要查看如何實作這些模式，請查看我們[UWP 範例應用程式](https://developer.microsoft.com/en-us/windows/samples):
- [BuildCast 影片播放程式](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler (午餐排程器)](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book (著色本)](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)