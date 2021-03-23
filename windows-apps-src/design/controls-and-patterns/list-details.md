---
description: 清單/詳細資料模式會顯示專案清單，以及目前所選項目的詳細資料。 這個模式通常用於電子郵件和連絡人清單/通訊錄。
title: 清單/詳細資料
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: List/details
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5532a0a5a5424d69c94d33db559aaa7afe786d83
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104806132"
---
# <a name="listdetails-pattern"></a>清單/詳細資料模式

清單/詳細資料模式具有 [清單] 窗格， (通常具有 [清單視圖](lists.md)) 和內容的詳細資料窗格。 當選取清單中的項目時，詳細資料窗格即會更新。 這個模式經常用於電子郵件和通訊錄。

> **重要 API**：[ListView 類別](/uwp/api/Windows.UI.Xaml.Controls.ListView)、[SplitView 類別](/uwp/api/windows.ui.xaml.controls.splitview)

![清單詳細資料模式範例](images/list-detail-pattern.png)

> [!TIP]
> 如果您想要使用 XAML 控制項來為您執行此模式，我們建議 Windows 社群工具組中的 [LISTDETAILSVIEW XAML 控制項](/windows/communitytoolkit/controls/masterdetailsview) 。

## <a name="is-this-the-right-pattern"></a>這是正確的模式嗎？

如果您想要執行下列作業，清單/詳細資料模式會很有作用：

- 建置電子郵件應用程式、通訊錄，或任何以清單詳細資料配置為基礎的應用程式。
- 找出大量內容並排定優先順序。
- 在內容之間往返工作時允許清單項目的快速新增和移除。

## <a name="choose-the-right-style"></a>選擇正確的樣式

在執行清單/詳細資料模式時，建議您根據可用的螢幕空間量，使用堆疊樣式或並列的樣式。

| 可用視窗寬度 | 建議樣式 |
|------------------------|-------------------|
| 320 epx-640 epx        | 堆疊           |
| 641 epx 或更寬       | 並存      |

## <a name="stacked-style"></a>堆疊樣式

在堆疊樣式中，一次只能顯示一個窗格：清單或詳細資料。

![堆疊模式的清單詳細資料](images/patterns-md-stacked.png)

使用者從 [清單] 窗格開始，然後選取清單中的專案以「向下切入」至 [詳細資料] 窗格。 對使用者而言，它看起來就像清單和詳細資料檢視都存在於兩個不同的頁面上。

### <a name="create-a-stacked-listdetails-pattern"></a>建立堆疊清單/詳細資料模式

建立堆疊清單/詳細資料模式的其中一種方式，是針對 [清單] 窗格和 [詳細資料] 窗格使用不同的頁面。 將清單視圖放在一個頁面上，然後將詳細資料窗格放在另一個頁面上。

![堆疊樣式清單詳細資料的元件](images/patterns-ld-stacked-parts.png)

針對清單視圖頁面， [清單視圖](lists.md) 控制項適合用來呈現可包含影像和文字的清單。

對於詳細資料檢視頁面，請使用最適合的[內容元素](../layout/layout-panels.md)。 如果您有許多個別的欄位，請考慮使用 **格線** 配置來將元素排列成表單。

如需在頁面之間瀏覽，請參閱[適用於 Windows 應用程式的瀏覽歷程記錄和向後瀏覽](../basics/navigation-history-and-backwards-navigation.md)。

## <a name="side-by-side-style"></a>並排樣式

在並列的樣式中，會同時顯示 [清單] 窗格和 [詳細資料] 窗格。

![清單/詳細資料模式](images/patterns-listdetail-400x227.png)

[清單] 窗格中的清單有選取的視覺效果，可指出目前選取的專案。 選取清單中的新專案會更新 [詳細資料] 窗格。

### <a name="create-a-side-by-side-listdetails-pattern"></a>建立並列清單/詳細資料模式

建立並列清單/詳細資料模式的其中一種方式是使用「 [分割視圖](split-view.md) 」控制項。 將清單視圖放在 [分割視圖] 窗格中，並在 [分割視圖] 內容中放置詳細資料檢視。

![列出詳細資料分割視圖元件](images/patterns-ld-splitview-parts.png)

針對 [清單] 窗格， [清單視圖](lists.md) 控制項適合用來呈現可包含影像和文字的清單。

對於詳細資料內容，請使用最適合的[內容元素](../layout/layout-panels.md)。 如果您有許多個別的欄位，請考慮使用 **格線** 配置來將元素排列成表單。

## <a name="adaptive-layout"></a>調適型配置

若要針對任何螢幕大小執行清單/詳細資料模式，請建立具有 [適應性](../layout/layouts-with-xaml.md)配置的回應式 UI。

![自我調整清單詳細資料版面配置](images/patterns_listdetail.png)

### <a name="create-an-adaptive-listdetails-pattern"></a>建立自我調整清單/詳細資料模式
若要建立調適型配置，請針對您的 UI 定義不同的 [**VisualStates**](/uwp/api/windows.ui.xaml.visualstate)，並使用 [**AdaptiveTriggers**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 宣告不同狀態的中斷點。

## <a name="get-the-sample-code"></a>取得範例程式碼

下列範例會使用調適型配置來執行清單/詳細資料模式，並示範靜態、資料庫和線上資源的資料系結： 
- [主要/詳細資料範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [ListView 和 GridView 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Windows Template Studio 主要/詳細資料範例](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [RSS 閱讀程式範例](https://github.com/Microsoft/Windows-appsample-rssreader)

> [!TIP]
> 如果您想要使用 XAML 控制項來為您執行此模式，我們建議 Windows 社群工具組中的 [LISTDETAILSVIEW XAML 控制項](/windows/communitytoolkit/controls/masterdetailsview) 。

## <a name="related-articles"></a>相關文章

- [清單](lists.md)
- [搜尋](search.md)
- [應用程式列和命令列](app-bars.md)
- [ListView 類別](/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [SplitView 類別](/uwp/api/windows.ui.xaml.controls.splitview)
