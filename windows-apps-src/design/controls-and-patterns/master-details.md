---
author: Jwmsft
Description: The master/detail pattern displays a master list and the details for the currently selected item. This pattern is frequently used for email and contact lists/address books.
title: 主要/詳細資料
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b30835e31e86c0c98d0c134ed28adca4413650c9
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7303558"
---
# <a name="masterdetails-pattern"></a>主要/詳細資料模式

 

主要/詳細資料模式具有一個主要窗格 (通常會有一個[清單檢視](lists.md)) 和內容的詳細資料窗格。 當選取主要清單中的項目時，會更新詳細資料窗格。 這個模式經常用於電子郵件和通訊錄。

> **重要 API**：[ListView 類別](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)、[SplitView 類別](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)

![主要/詳細資料模式的範例](images/HIGSecOne_MasterDetail.png)

## <a name="is-this-the-right-pattern"></a>這是正確的模式嗎？

在下列情況中，主要/詳細資料模式都能運作：

-   建置電子郵件應用程式、通訊錄，或任何以清單詳細資料配置為基礎的應用程式。
-   找出大量內容並排定優先順序。
-   在內容之間往返工作時允許清單項目的快速新增和移除。

## <a name="choose-the-right-style"></a>選擇正確的樣式

實作主要/詳細資料模式時，建議您根據可用的螢幕空間量，使用堆疊樣式或並排樣式。

| 可用視窗寬度 | 建議樣式 |
|------------------------|-------------------|
| 320 epx-640 epx        | 堆疊           |
| 641 epx 或更寬       | 並排      |

 
## <a name="stacked-style"></a>堆疊樣式

在堆疊樣式中，一次只能看到一個檢視：主要或詳細資料檢視。

![堆疊模式中的主要詳細資料](images/patterns-md-stacked.png)

使用者開始於主要窗格，並透過選取主要清單中的項目，「向下探查」到詳細資料窗格。 對使用者而言，主要與詳細資料檢視似乎是存在於兩個個別頁面上。

### <a name="create-a-stacked-masterdetails-pattern"></a>建立堆疊的主要/詳細資料模式

建立堆疊的主要/詳細資料模式的一種方式是針對主要窗格和詳細資料窗格使用不同的頁面。 將主要檢視放在一個頁面，然後將詳細資料窗格放在另一個頁面。

![堆疊樣式主要詳細資料的組件](images/patterns-md-stacked-parts.png)

對於主要檢視頁面，[清單檢視](lists.md)非常適合用來呈現可以包含影像及文字的清單。 

對於詳細資料檢視頁面，請使用最適合的[內容元素](../layout/layout-panels.md)。 如果您有許多個別的欄位，請考慮使用**格線**配置將元素排列成表單。

如需在頁面之間瀏覽，請參閱[適用於 UWP app 的瀏覽歷程記錄和向後瀏覽](../basics/navigation-history-and-backwards-navigation.md)。

## <a name="side-by-side-style"></a>並排樣式

在並排樣式中，可同時看見主要窗格和詳細資料窗格。

![主要/詳細資料模式](images/patterns-masterdetail-400x227.png)

主要窗格中的清單具有用來表示目前選取項目的視覺化選取。 在主要清單中選取新項目會更新詳細資料窗格。

### <a name="create-a-side-by-side-masterdetails-pattern"></a>建立並排主要/詳細資料模式

有一種建立並排主要/詳細資料模式的方式是使用[分割檢視](split-view.md)控制項。 將主要檢視放在分割檢視窗格中，並將詳細資料檢視放在分割檢視內容中。

![主要/詳細資料分割檢視組件](images/patterns_md_splitview_parts.png)

對於主要窗格，[清單檢視](lists.md)非常適合用來呈現可以包含影像和文字的清單。

對於詳細資料內容，請使用最適合的[內容元素](../layout/layout-panels.md)。 如果您有許多個別的欄位，請考慮使用**格線**配置將元素排列成表單。

## <a name="adaptive-layout"></a>調適型配置

若要實作適用於任何螢幕大小的主要/詳細資料模式，請建立採取[調適型配置](../layout/layouts-with-xaml.md)的回應式 UI。

![調適型主要/詳細資料配置](images/patterns_masterdetail.png)

### <a name="create-an-adaptive-masterdetails-pattern"></a>建立調適型主要/詳細資料模式
若要建立調適型配置，請針對您的 UI 定義不同的 [**VisualStates**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.visualstate)，並使用 [**AdaptiveTriggers**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 宣告不同狀態的中斷點。

## <a name="get-the-sample-code"></a>取得範例程式碼

下列範例使用調適型配置實作主要/詳細資訊模式，並示範靜態、資料庫及離線資源的資料繫結： 
- [主要/詳細資料範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [主要/詳細資料以及選取項目範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Windows Template Studio 主要/詳細資料範例](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [RSS 閱讀程式範例](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>相關文章

- [清單](lists.md)
- [搜尋](search.md)
- [應用程式列和命令列](app-bars.md)
- [ListView 類別](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [SplitView 類別](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)
