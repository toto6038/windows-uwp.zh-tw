---
Description: 在通用 Windows 平台 (UWP) app 中的瀏覽是以瀏覽結構、瀏覽元素和系統層級功能的彈性模型為基礎。
title: UWP app 的瀏覽基本知識
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1c764eeb57ec8046a93e7fb58e156fa68daea8df
ms.sourcegitcommit: 13fe5d04bdb43c75d0fc4de18c2c3d4ae58ff982
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2019
ms.locfileid: "64564519"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP app 的瀏覽設計基本知識

![瀏覽基本知識標頭](images/nav/navigation-basics-header.jpg)

如果您將 App 想成是頁面的集合，則*瀏覽*描述的是頁面之間和頁面內的移動動作。 這是使用者體驗的起點，也是使用者尋找感興趣的功能與內容的方式。 它非常重要，也可能難以正確開始。

我們有非常多的瀏覽選擇。 我們可以：

:::row:::
    :::column:::
        ![navigation example 1](images/nav/nav-1.svg)

要求使用者通過一連串的頁面順序。
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

提供可讓使用者直接跳到任何頁面的功能表。
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

將所有項目放在單一頁面上，並提供篩選的機制，可檢視內容。
    :::column-end:::
:::row-end:::

當沒有單一的瀏覽設計可適用於每一個 App 時，倒是有原則和指導方針可幫助您決定適合您 App 的設計。

## <a name="principles-of-good-navigation"></a>良好瀏覽的原則

讓我們從良好瀏覽設計的原則開始：

- **一致性：** 符合使用者的期望。
- **簡易性：** 不會進行比您需要更多。
- **清楚：** 提供明確的路徑和選項。

### <a name="consistency"></a>一致性

瀏覽應該與使用者的期望一致。 使用[標準控制項](#use-the-right-controls)使用者都是針對圖示的位置，熟悉並遵循標準慣例，樣式會瀏覽可預測且直覺式的使用者。

![頁面元件影像](images/nav/page-components.svg)

> 使用者預期會在標準位置尋找特定 UI 元素。

### <a name="simplicity"></a>簡潔

瀏覽項目越少，使用者做出決定就越簡單。 讓使用者輕鬆抵達重要的目的地，並隱藏較不重要的項目，可幫助使用者更快到達想要的位置。

:::row:::
    :::column:::
        ![do example](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

提供熟悉的瀏覽功能表中的導覽項目。
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

具有許多導覽選項的使用者會拖垮。
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>清楚明確

清楚的路徑可讓使用者邏輯瀏覽。 提供清楚明瞭的選項，並釐清頁面之間的關係，可防止使用者迷失方向。

![執行範例](images/nav/clarity-image.svg)

> 將目的地清楚標示出，讓使用者知道自己身在何處。

## <a name="general-recommendations"></a>一般建議

現在來看看我們的設計原則 - 一致性、簡潔和清楚明確 - 並使用它們來想出一些一般建議。

1. 考量您的使用者。 找出使用者可能在您的 app 中依循的一般途徑，並思考使用者來到每個頁面的原因，以及接著想前往哪裡。

2. 應避免深入導覽階層。 如果您的瀏覽超過三層，可能會讓您的使用者陷在深度階層中而難以離開。

3. 避免「上下彈跳」。 當有相關的內容時，但瀏覽到該內容卻要求使用者往上一層然後再往下一層，就發生上下彈跳狀況。

## <a name="use-the-right-structure"></a>使用適當的結構

現在您已熟悉一般瀏覽原則，那麼接下來要如何建構您的 app？ 有兩個一般結構︰單層式和階層式。

:::row:::
    :::column:::
        ![Pages arranged in a flat structure](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Flat/lateral

在單層式/側面結構中，頁面是並排存在。 您可以依任何順序從一個頁面瀏覽到另一個。

我們建議使用單層式結構的狀況是︰

- 頁面可以用任何順序瀏覽。
- 頁面之間有清楚的區別，且沒有明顯的父系/子系關係。
- 在群組中有少於 8 個的頁面。 <br>
(有多個頁面時，使用者可能會難以了解頁面的獨特性，或難以了解他們目前在群組中的位置。 如果您認為這對您的應用程式不構成問題，那請將頁面以對等方式排列。 否則，請考慮使用階層結構將頁面分成兩組，或分成多個較小的群組。)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Pages arranged in a hierarchy](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Hierarchical

在階層式結構中，頁面是組織成類似樹狀的結構。 每個子頁面都有一個父頁面，但父頁面可以有一或多個子頁面。 若要進入子頁面，您必須經過父頁面。

階層結構對於組織跨多個頁面的複雜內容來說很實用。 缺點是會有一些瀏覽負荷︰結構越深層，就需要多按幾下才能在頁面間瀏覽。

我們建議一個階層式結構的時機：
        
- 頁面應以特定順序周遊。
- 頁面之間有清楚的父-子關係。
- 群組中的頁面數超過 7 個。
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![an app with a hybrid structure](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### Combining structures

您沒有選擇一個結構或其他;同時使用許多格式設計應用程式。 App 可在最上層頁面使用單層式結構，可以任何順序檢視頁面，而具有較複雜關係的頁面則採用階層式結構。

如果您的瀏覽結構有多個層級，我們建議讓對等瀏覽元素只連結到其目前所在子樹系中的對等元素。 請考慮相鄰的圖例中，有兩個層級的導覽結構會顯示：

- 於層級 1，對等瀏覽元素應提供對頁面 A、B、C 和 D 的存取功能。
- 在層級 2 中，A2 頁面的對等瀏覽元素應只連結到其他 A2 頁面。 它們不應連結到 C 子樹系中的層級 2 頁面。
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>使用適當的控制項

一旦您決定頁面結構之後，您必須決定使用者瀏覽頁面的方式。 UWP 提供各種不同的瀏覽控制項，有助於確保您的 App 提供一致、可靠的瀏覽體驗。

:::row:::
    :::column:::
        ![Frame image](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

有幾個例外，即任何有多個頁面的 App 都會使用框架。 一般來說，App 會有主要頁面，包含框架和主要瀏覽元素，例如瀏覽檢視控制項。 當使用者選取頁面時，框架會載入並且顯示。
:::row-end:::

:::row:::
    :::column:::
        ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Top navigation and tabs**](../controls-and-patterns/navigationview.md)

顯示相同層級中水平的頁面連結清單。 [NavigationView](../controls-and-patterns/navigationview.md)控制實作頂端導覽列與索引標籤模式。
        
使用上方導覽時：

- 您想要顯示在螢幕上的所有導覽選項。
- 您想要為您的應用程式內容的更多空間。
- 圖示無法清楚地描述您的瀏覽類別。
        
使用索引標籤的時機：

- 您想要保留瀏覽歷程記錄和網頁的狀態。
- 您預期使用者經常索引標籤之間切換。

:::row-end:::

:::row:::
    :::column:::
         ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
        :::column span="2":::
    [**Pivot**](../controls-and-patterns/pivot.md)
    
類似於[瀏覽檢視](../controls-and-patterns/navigationview.md)，但僅提供針對觸控和稍有不同的瀏覽行為的其他支援。
    
使用 pivot 時：
- 您想要允許兩個類別之間撥動觸控應用程式
- 您要旋轉木馬 infintely 瀏覽選項
- 您不需要有效掌控類別之間的導覽行為

:::row-end:::

:::row:::
    :::column:::
        ![navview image](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Left navigation**](../controls-and-patterns/navigationview.md)

顯示連結到最上層頁面的垂直清單。 使用時機：
        
- 頁面位於最上層。
- 有許多的導覽項目 (超過 5)
- 您預期使用者不會在頁面之間頻繁切換。

:::row-end:::
        
:::row:::
    :::column:::
        ![Master details image](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Master/details**](../controls-and-patterns/master-details.md)

顯示項目的清單 (主要檢視)。 選取在詳細區段中顯示其相對應頁面的項目。 使用時機：
        
- 您預期使用者會在子項目之間頻繁切換。
- 您想讓使用者對個別項目或項目群組執行高層級操作 (例如刪除或排序)，並且也想讓使用者能夠檢視或更新每個項目的詳細資料。

主要/詳細資料適合電子郵件收件匣、連絡人清單及資料項目。
:::row-end:::

:::row:::
    :::column:::
        ![Hyperlinks and buttons image](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**Hyperlinks**](../controls-and-patterns/hyperlinks.md)

內嵌瀏覽元素可顯示在頁面的內容中。 不同於其他瀏覽元素在頁面間皆須保持一致，內嵌於內容的瀏覽元素會隨頁面不同而改變。
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>下一步:將巡覽程式碼新增至您的應用程式

下一篇文章：[實作基本瀏覽](navigate-between-two-pages.md)，顯示所需的程式碼，才能使用框架控制項在您的 App 中的兩個頁面之間啟用基本瀏覽。
