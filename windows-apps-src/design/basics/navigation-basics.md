---
description: Windows 應用程式中的瀏覽，以瀏覽結構、瀏覽元素和系統層級功能的彈性模型為基礎。
title: Windows 應用程式的瀏覽基本知識
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 36a5489ab4fa278163fd5fad53be4d28156d4cf4
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804455"
---
# <a name="navigation-design-basics-for-windows-apps"></a>Windows 應用程式的瀏覽設計基本知識

![瀏覽基本知識標頭](images/nav/navigation-basics-header.jpg)

如果您將應用程式想成是頁面的集合，則 *瀏覽* 描述的是頁面之間和頁面內的移動動作。 這是使用者體驗的起點，也是使用者尋找感興趣的功能與內容的方式。 它非常重要，也可能難以正確開始。

我們有非常多的瀏覽選擇。 我們可以：

:::row:::
    :::column:::
        ![瀏覽範例 1](images/nav/nav-1.svg)

要求使用者依序瀏覽一系列頁面。
    :::column-end:::
    :::column:::
        ![瀏覽範例 2](images/nav/nav-2.svg)

提供可讓使用者直接跳到任何頁面的功能表。
    :::column-end:::
    :::column:::
        ![瀏覽範例 3](images/nav/nav-3.svg)

將所有東西放在單一頁面上，並提供檢視內容的篩選機制。
    :::column-end:::
:::row-end:::

當沒有單一的瀏覽設計可適用於每個應用程式時，倒是有原則和指導方針可幫助您決定適合您應用程式的設計。

## <a name="principles-of-good-navigation"></a>良好瀏覽的原則

讓我們從良好瀏覽設計的原則開始：

- **一致性：** 符合使用者期望。
- **簡單︰** 不超過您的需求。
- **明確：** 提供清楚的路徑和選項。

### <a name="consistency"></a>一致性

瀏覽應該與使用者的期望一致。 使用使用者熟悉的[標準控制項](#use-the-right-controls)，並依循圖示、位置和樣式的標準慣例，瀏覽對使用者來說就是可預測且直覺。

![頁面元件影像](images/nav/page-components.svg)

> 使用者預期會在標準位置尋找特定 UI 元素。

### <a name="simplicity"></a>簡潔

瀏覽項目越少，使用者做出決定就越簡單。 讓使用者輕鬆抵達重要的目的地，並隱藏較不重要的項目，可幫助使用者更快到達想要的位置。

:::row:::
    :::column:::
        ![綠色長條的第一個螢幕擷取畫面，內有綠色核取記號和 Do 一字。](images/nav/do.svg)

        ![良好的 navview](images/nav/navview-good.svg)

提供熟悉的瀏覽功能表中的瀏覽項目。
    :::column-end:::
    :::column:::
        ![禁止事項範例](images/nav/dont.svg)

        ![不良的 navview](images/nav/navview-bad.svg)

具有許多瀏覽選項的使用者會拖垮。
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>明確

清楚的路徑可讓使用者邏輯瀏覽。 提供清楚明瞭的選項，並釐清頁面之間的關係，可防止使用者迷失方向。

![應用程式模擬的螢幕擷取畫面，其中顯示使用者的明確瀏覽路徑。](images/nav/clarity-image.svg)

> 將目的地清楚標示出，讓使用者知道自己身在何處。

## <a name="general-recommendations"></a>一般建議

現在來看看我們的設計原則 - 一致性、簡潔和明確 - 並使用它們來想出一些一般建議。

1. 考量您的使用者。 找出使用者可能在您的應用程式中依循的一般途徑，並思考使用者來到每個頁面的原因，以及接著想前往哪裡。

2. 避免深層的瀏覽階層。 如果您的瀏覽超過三層，可能會讓您的使用者陷在深度階層中而難以離開。

3. 避免「上下彈跳」。 當有相關的內容時，但瀏覽到該內容卻要求使用者往上一層然後再往下一層，就發生上下彈跳狀況。

## <a name="use-the-right-structure"></a>使用適當的結構

現在您已熟悉一般瀏覽原則，那麼接下來要如何建構您的應用程式？ 有兩個一般結構︰單層式和階層式。

:::row:::
    :::column:::
        ![單層式結構排列的頁面](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="flatlateral"></a>單層式/側面

在單層式/側面結構中，頁面是並排存在。 您可以依任何順序從一個頁面瀏覽到另一個。

我們建議使用單層式結構的狀況是︰

- 頁面可以用任何順序瀏覽。
- 頁面之間有清楚的區別，且沒有明顯的父系/子系關係。
- 群組中的頁面數超過 8 個。 <br>
(有多個頁面時，使用者可能會難以了解頁面的獨特性，或難以了解他們目前在群組中的位置。 如果您認為這對您的應用程式不構成問題，那請將頁面以對等方式排列。 否則，請考慮使用階層結構將頁面分成兩組，或分成多個較小的群組。)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![階層排列的頁面](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="hierarchical"></a>階層式

在階層式結構中，頁面是組織成類似樹狀的結構。 每個子頁面都有一個父頁面，但父頁面可以有一或多個子頁面。 若要進入子頁面，您必須經過父頁面。

階層結構對於組織跨多個頁面的複雜內容來說很實用。 缺點是會有一些瀏覽負荷︰結構越深層，就需要多按幾下才能在頁面間瀏覽。

我們建議使用階層式結構的狀況是︰
        
- 頁面應以特定順序周遊。
- 頁面之間有清楚的父子關係。
- 群組中的頁面數超過 7 個。
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![具備混合式結構的 app](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="combining-structures"></a>結合結構

您不是只能選擇一種結構；許多妥善設計的應用程式會使用二者。 應用程式可在最上層頁面使用單層式結構，可以任何順序檢視頁面，而具有較複雜關係的頁面則採用階層式結構。

如果您的瀏覽結構有多個層級，我們建議讓對等瀏覽元素只連結到其目前所在子樹系中的對等元素。 請思考相鄰的圖例，其中顯示具有兩個層級的瀏覽結構：

- 在層級 1，對等瀏覽元素應提供對頁面 A、B、C 和 D 的存取功能。
- 在層級 2 中，A2 頁面的對等瀏覽元素應只連結到其他 A2 頁面。 它們不應連結到 C 子樹系中的層級 2 頁面。
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>使用適當的控制項

一旦您決定頁面結構之後，您必須決定使用者瀏覽頁面的方式。 UWP 提供各種不同的瀏覽控制項，有助於確保您的應用程式提供一致、可靠的瀏覽體驗。

:::row:::
    :::column:::
        ![框架影像](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**框架**](/uwp/api/Windows.UI.Xaml.Controls.Frame)

有幾個例外，即任何有多個頁面的應用程式都會使用框架。 一般來說，應用程式會有主要頁面，包含框架和主要瀏覽元素，例如瀏覽檢視控制項。 當使用者選取頁面時，框架會載入並且顯示。
:::row-end:::

:::row:::
    :::column:::
        ![索引標籤和樞紐影像](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**頂端瀏覽**](../controls-and-patterns/navigationview.md)

顯示相同層級中水平的頁面連結清單。 [NavigationView](../controls-and-patterns/navigationview.md) 控制項會實作頂端瀏覽模式。
        
在以下情況使用上方瀏覽：

- 您想要在螢幕上顯示所有的瀏覽選項。
- 您希望您的應用程式內容有更多空間。
- 圖示無法清楚地描述您的瀏覽類別。

:::row-end:::

:::row:::
    :::column:::
        ![索引標籤和樞紐影像](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**索引標籤**](../controls-and-patterns/tab-view.md)

顯示一組水平的索引標籤及其各自的內容。 [TabView](../controls-and-patterns/tab-view.md) 控制項可用來既顯示數個頁面 (或文件)，同時又讓使用者能夠重新排列、開啟或關閉索引標籤。
    
在以下情況使用索引標籤：

- 您想要讓使用者能夠動態開啟、關閉或重新排列索引標籤。
- 您預期可能會有一次開啟大量索引標籤的情形。
- 您希望使用者能夠輕鬆地在使用索引標籤的應用程式中，於視窗之間移動索引標籤，情形類似 Microsoft Edge 等網頁瀏覽器。

:::row-end:::

:::row:::
    :::column:::
        ![navview 影像](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**左方瀏覽**](../controls-and-patterns/navigationview.md)

顯示連結到最上層頁面的垂直清單。 使用時機：
        
- 頁面位於最上層。
- 有許多瀏覽項目 (超過 5 個)
- 您預期使用者不會在頁面之間頻繁切換。

:::row-end:::
        
:::row:::
    :::column:::
        ![列出詳細資料影像](images/nav/thumbnail-list-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**清單/詳細資料**](../controls-and-patterns/list-details.md)

顯示專案清單。 選取在詳細區段中顯示其相對應頁面的項目。 使用時機：
        
- 您預期使用者會在子項目之間頻繁切換。
- 您想讓使用者對個別項目或項目群組執行高層級操作 (例如刪除或排序)，並且也想讓使用者能夠檢視或更新每個項目的詳細資料。

清單/詳細資料非常適合電子郵件收件匣、連絡人清單和資料輸入。
:::row-end:::

:::row:::
    :::column:::
        ![超連結和按鈕影像](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**超連結**](../controls-and-patterns/hyperlinks.md)

內嵌瀏覽元素可顯示在頁面的內容中。 不同於其他瀏覽元素在頁面間皆須保持一致，內嵌於內容的瀏覽元素會隨頁面不同而改變。
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>接下來︰新增瀏覽程式碼到您的應用程式

下一篇文章：[實作基本瀏覽](navigate-between-two-pages.md)，顯示所需的程式碼，才能使用框架控制項在您應用程式中的兩個頁面之間啟用基本瀏覽。
