---
title: 適用於 Windows 應用程式的頁面配置
description: 設計您的應用程式時，首先要考慮的事項是版面配置結構。 本文涵蓋基本頁面配置的通用結構，包括您需要的 UI 元素，以及它們在頁面上的位置。 在 Windows 應用程式中，每個頁面通常都有瀏覽、命令和內容元素。
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: 238f341373e08f2ae73b22a01020e37e4fe68673
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804442"
---
# <a name="page-layout"></a>頁面配置

在 Windows 應用程式中，每個 [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) (頁面) 通常都有瀏覽、命令和內容元素。 

您的應用程式可以有多個頁面：當使用者啟動 Windows 應用程式時，應用程式程式碼會建立一個 [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) (框架)，放在應用程式的 [**Window**](/uwp/api/windows.ui.xaml.window) (視窗) 中。 然後，Frame 可以在應用程式的 [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) 實例之間 [導覽](../basics/navigate-between-two-pages.md)。 

大部分的頁面都遵循通用的版面配置結構，本文涵蓋您需要的 UI 元素，以及它們在頁面上的位置。 

![頁面結構](images/page-components.svg)

## <a name="navigation"></a>瀏覽
您的應用程式版面配置是從您選擇的導覽模型開始，這會定義您的使用者如何在應用程式的頁面之間導覽。 在本文中，我們將討論兩個常見的導覽模式：左側導覽和頂端導覽。 如需選擇其他瀏覽選項的指引，請參閱 [Windows 應用程式的瀏覽設計基本知識](../basics/navigation-basics.md)。

![頂端和左側導覽模式](images/top-left-nav.svg)

### <a name="left-nav"></a>左側導覽
左側導覽 (或稱為[導覽窗格](../controls-and-patterns/navigationview.md)) 模式通常會保留給應用程式層級的導覽，且在應用程式中為最高層級，這表示它應該一律為可見且可供使用。 當您的應用程式中有超過五個導覽項目或超過五頁時，建議您使用左側導覽。 導覽窗格模式通常包含：
- 瀏覽項目
- 進入應用程式設定的進入點
- 進入帳戶設定的進入點

[NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) 控制項會實作 UWP 的左側導覽模式。

選取導覽項目時，Frame 應該導覽至選取項目的 Page。

![展開的導覽窗格](images/navview-expanded.svg)

功能表按鈕可讓使用者展開和摺疊導覽窗格。 當螢幕大小大於 640 像素時，按一下功能表按鈕會將導覽窗格摺疊為細條。

![精簡的導覽窗格](images/navview-compact.svg)

當螢幕大小小於 640 像素時，導覽窗格會完全摺疊。

![最小的導覽窗格](images/navview-minimal.svg)

### <a name="top-nav"></a>頂端導覽

頂端導覽也可以做為最上層導覽。 左側導覽可摺疊，但頂端導覽一律是可見的。 [NavigationView](../controls-and-patterns/navigationview.md) 控制項會實作 UWP 的頂端導覽與索引標籤模式。

:::image type="content" source="../controls-and-patterns/images/displaymode-top.png" alt-text="頂端導覽":::

## <a name="command-bar"></a>命令列

接下來，您可能想讓使用者輕鬆存取您的應用程式最常用的工作。 [命令列](../controls-and-patterns/app-bars.md)可以存取應用程式層級或頁面層級命令，而且可以搭配任何導模式使用。

![命令列位於頂端 ](images/app-bar-desktop.svg)

命令列可以放在頁面的頂端或底部，視何者最適合您的應用程式。

![命令列位於底部](images/app-bar-mobile.svg)

## <a name="content"></a>內容

最後，內容會因應用程式而有很大的差異，因此您可以用許多不同的方式呈現內容。 在此，我們會說明一些您可能想要在應用程式中使用的常見頁面模式。 許多應用程式會使用這些常見頁面模式的一部分或全部，來顯示不同類型的內容。 同樣地，這些模式可以混搭使用，創造出最佳的應用程式。

## <a name="landing"></a>登陸

![登陸頁面](images/hero-screen.svg)

登陸頁面，也就是主題畫面，通常出現在應用程式體驗的最上層。 大型介面區域做為應用程式的舞台，強調出使用者會想要瀏覽和使用的內容。

## <a name="collections"></a>集合

![圖庫](images/gridview.svg)

集合可讓使用者瀏覽內容或資料群組。 [方格檢視](../controls-and-patterns/item-templates-gridview.md)是相片或媒體為主內容的理想選擇，而[清單檢視](../controls-and-patterns/item-templates-listview.md)是文字為主內容或資料的理想選擇。

## <a name="listdetail"></a>清單/詳細資料

![清單詳細資料](images/master-detail.svg)

[清單/詳細資料](../controls-and-patterns/list-details.md)模型是由清單視圖和內容視圖組成 (詳細資料) 。 這兩個窗格都是固定的，並可垂直捲動。 當使用者選取清單檢視中的一個項目，內容檢視會隨之更新。

## <a name="forms"></a>表單
![顯示空白文字方塊和按鈕的表單螢幕擷取畫面。](images/form.svg)

[表單](../controls-and-patterns/forms.md)是一組控制項，會收集和提交使用者的資料。 大多數 (即使不是全部) 應用程式都會在設定頁面上、登入入口網站、意見反應中樞、帳戶建立或基於其他目的使用某種表單。 

## <a name="sample-apps"></a>範例應用程式
若要了解如何執行這些模式，請看我們的 [Windows 範例應用程式](https://developer.microsoft.com/windows/samples)：
- [BuildCast 影片播放器](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler (午餐排程器)](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book (著色本)](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
