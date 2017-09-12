---
author: Jwmsft
Description: "主要/詳細資料模式會顯示主要清單和目前所選項目的詳細資料。 這個模式通常用於電子郵件和連絡人清單/通訊錄。"
title: "主要/詳細資料"
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 49a586aac0c846cdad02f8448532238bd3eb8551
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2017
---
# <a name="masterdetails-pattern"></a>主要/詳細資料模式

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

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
| 320 epx-719 epx        | 堆疊           |
| 720 epx 或更寬       | 並排      |

 
## <a name="stacked-style"></a>堆疊樣式

在堆疊樣式中，一次只能看到一個檢視：主要或詳細資料檢視。

![堆疊模式中的主要詳細資料](images/patterns-md-stacked.png)

使用者開始於主要窗格，並透過選取主要清單中的項目，「向下探查」到詳細資料窗格。 對使用者而言，主要與詳細資料檢視似乎是存在於兩個個別頁面上。

### <a name="create-a-stacked-masterdetails-pattern"></a>建立堆疊的主要/詳細資料模式

建立堆疊的主要/詳細資料模式的一種方式是針對主要窗格和詳細資料窗格使用不同的頁面。 將提供主要清單的清單檢視放在某一個頁面，然後將詳細窗格的內容元素放在另一個頁面。

![堆疊樣式主要詳細資料的組件](images/patterns-md-stacked-parts.png)

針對主要窗格，[清單檢視](lists.md)能夠用於呈現可以包含影像和文字的清單。

針對詳細資料窗格，請使用最適合的內容元素。 如果您有許多個別的欄位，請考慮使用格線配置來將元素排列成表單。

## <a name="side-by-side-style"></a>並排樣式

在並排樣式中，可同時看見主要窗格和詳細資料窗格。

![主要/詳細資料模式](images/patterns-masterdetail-400x227.png)

主要窗格中的清單具有用來表示目前選取項目的視覺化選取。 在主要清單中選取新項目會更新詳細資料窗格。

### <a name="create-a-side-by-side-masterdetails-pattern"></a>建立並排主要/詳細資料模式

針對主要窗格，[清單檢視](lists.md)能夠用於呈現可以包含影像和文字的清單。

針對詳細資料窗格，請使用最適合的內容元素。 如果您有許多個別的欄位，請考慮使用格線配置來將元素排列成表單。

## <a name="get-the-code-samples"></a>取得程式碼範例

如需顯示主要/詳細資料模式的範例程式碼，請參閱下列範例： 

- [客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 
- [ListView 和 GridView 範例](http://go.microsoft.com/fwlink/p/?LinkId=619900)
- [RSS 閱讀程式範例](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>相關文章

- [清單](lists.md)
- [搜尋](search.md)
- [應用程式列和命令列](app-bars.md)
- [ListView 類別](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)
