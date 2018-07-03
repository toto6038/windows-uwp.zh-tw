---
author: serenaz
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: UWP app 的瀏覽基本知識
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0980f24394a075596b60e4a8005b303857b91304
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843068"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP app 的瀏覽設計基本知識

![瀏覽基本知識標頭](images/nav/navigation-basics-header.jpg)

如果您將 App 想成是頁面的集合，則*瀏覽*描述的是頁面之間和頁面內的移動動作。 這是使用者體驗的起點，也是使用者尋找感興趣的功能與內容的方式。 它非常重要，也可能難以正確開始。

我們有非常多的瀏覽選擇。 我們可以：

::: 列:::::: 欄:::![瀏覽範例 1](images/nav/nav-1.svg)

        Require users to go through a series of pages in order.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

        Provide a menu that allows users to jump directly to any page.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

        Place everything on a single page and provide filtering mechanisms for viewing content.
    :::column-end:::
:::列結束:::

當沒有單一的瀏覽設計可適用於每一個 App 時，倒是有原則和指導方針可幫助您決定適合您 App 的設計。

## <a name="principles-of-good-navigation"></a>良好瀏覽的原則

讓我們從良好瀏覽設計的原則開始：

- **一致性︰** 符合使用者的期望。
- **簡單︰** 不超過您的需求。
- **明確：** 提供清楚的路徑和選項。

### <a name="consistency"></a>一致性

瀏覽應該與使用者的期望一致。 使用使用者熟悉的[標準控制項](#use-the-right-controls)，並依循圖示、位置和樣式的標準慣例，瀏覽對使用者來說就是可預測且直覺。

![頁面元件影像](images/nav/page-components.svg)

> 使用者預期會在標準位置尋找特定 UI 元素。

### <a name="simplicity"></a>簡潔

瀏覽項目越少，使用者做出決定就越簡單。 讓使用者輕鬆抵達重要的目的地，並隱藏較不重要的項目，可幫助使用者更快到達想要的位置。

::: 列:::::: 欄:::![執行範例](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

        Present navigation items in a familiar navigation menu.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

        Constantly provide many navigation options to overwhelm the user.
    :::column-end:::
:::列結束:::

### <a name="clarity"></a>清楚明確

清楚的路徑可讓使用者邏輯瀏覽。 提供清楚明瞭的選項，並釐清頁面之間的關係，可防止使用者迷失方向。

![執行範例](images/nav/clarity-image.svg)

> 將目的地清楚標示出，讓使用者知道自己身在何處。

## <a name="general-recommendations"></a>一般建議

現在來看看我們的設計原則 - 一致性、簡潔和清楚明確 - 並使用它們來想出一些一般建議。

1. 考量您的使用者。 找出使用者可能在您的 app 中依循的一般途徑，並思考使用者來到每個頁面的原因，以及接著想前往哪裡。 

2. 避免深層的瀏覽階層。 如果您的瀏覽超過三層，可能會讓您的使用者陷在深度階層中而難以離開。

3. 避免「上下彈跳」。 當有相關的內容時，但瀏覽到該內容卻要求使用者往上一層然後再往下一層，就發生上下彈跳狀況。

## <a name="use-the-right-structure"></a>使用適當的結構

現在您已熟悉一般瀏覽原則，那麼接下來要如何建構您的 app？ 有兩個一般結構︰單層式和階層式。

:::列::: :::欄::: ![以單層式結構排列頁面](images/nav/flat-lateral-structure.svg) :::欄結束::: :::欄範圍="2":::
        ### Flat/lateral

        In a flat/lateral structure, pages exist side-by-side. You can go from on page to another in any order. 

        We recommend using a flat structure when:
        <ul>
        <li>The pages can be viewed in any order.</li>
        <li>The pages are clearly distinct from each other and don't have an obvious parent/child relationship.</li>
        <li>There are fewer than 8 pages in the group.<br/>
        (When there are more pages, it might be difficult for users to understand how the pages are unique or to understand their current location within the group. If you don't think that's an issue for your app, go ahead and make the pages peers. Otherwise, consider using a hierarchical structure to break the pages into two or more smaller groups.)</li>
        </ul>

    :::column-end:::
:::列結束:::

:::列::: :::欄::: ![以階層排列頁面](images/nav/hierarchical-structure.svg) :::欄結束::: :::欄範圍="2":::
        ### Hierarchical

        In a hierarchical structure, pages are organized into a tree-like structure. Each child page has one parent, but a parent can have one or more child pages. To reach a child page, you travel through the parent.

        Hierarchical structures are good for organizing complex content that spans lots of pages. The downside is some navigation overhead: the deeper the structure, the more clicks it takes to get from page to page. 

        We recommend a hiearchical structure when:
        <ul>
        <li>Pages should be traversed in a specific order.</li>
        <li>There is a clear parent-child relationship between pages.</li>
        <li>There are more than 7 pages in the group.</li>
        </ul>
    :::column-end:::
:::列結束:::

:::列::: :::欄::: ![具備混合式結構的 App](images/nav/combining-structures.svg) :::欄結束::: :::欄範圍="2":::
        ### Combining structures

        You don't have choose to one structure or the other; many well-design apps use both. An app can use flat structures for top-level pages that can be viewed in any order, and hierarchical structures for pages that have more complex relationships.

        If your navigation structure has multiple levels, we recommend that peer-to-peer navigation elements only link to the peers within their current subtree. Consider the adjacent illustration, which shows a navigation structure that has two levels:

        - At level 1, the peer-to-peer navigation element should provide access to pages A, B, C, and D.
        - At level 2, the peer-to-peer navigation elements for the A2 pages should only link to the other A2 pages. They should not link to level 2 pages in the C subtree. 
    :::column-end:::
:::列結束:::

## <a name="use-the-right-controls"></a>使用適當的控制項

一旦您決定頁面結構之後，您必須決定使用者瀏覽頁面的方式。 UWP 提供各種不同的瀏覽控制項，有助於確保您的 App 提供一致、可靠的瀏覽體驗。 

我們建議您根據 App 中的瀏覽元素數目選取瀏覽控制項。 如果您有五個或更少瀏覽項目，則使用最上層瀏覽，像是[索引標籤和樞紐](../controls-and-patterns/tabs-pivot.md)。 如果您有六個以上的瀏覽項目，則使用左側瀏覽，像是[瀏覽檢視](../controls-and-patterns/navigationview.md)或[主要/詳細資料](../controls-and-patterns/master-details.md)。

:::列::: :::欄::: ![畫面影像](images/nav/thumbnail-frame.svg) :::欄結束::: :::欄範圍="2"::: <a href="https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame"><b>畫面</b></a>

        With few exceptions, any app that has multiple pages uses a frame. Typically, an app has a main page that contains the frame and a primary navigation element, such as a navigation view control. When the user selects a page, the frame loads and displays it.
:::列結束:::

::: 列:::::: 欄:::![索引標籤和樞紐影像](images/nav/thumbnail-tabs-pivot.svg)::: 欄結束:::::: 欄範圍="2"::: <a href="../controls-and-patterns/tabs-pivot.md"><b>索引標籤和樞紐</b></a><br>

        Displays a horizontal list of links to pages at the same level. Use when:
        <ul>
        <li>There are 2-5 pages. (You can use tabs/pivots when there are more than 5 pages, but it might be difficult to fit all the tabs/pivots on the screen.)</li>
        <li>You expect users to switch between pages frequently.</li>
        </ul>
:::列結束:::

::: 列:::::: 欄:::![索引標籤和樞紐影像](images/nav/thumbnail-navview.svg)::: 欄結束:::::: 欄範圍="2"::: <a href="../controls-and-patterns/navigationview.md"><b>瀏覽檢視</b></a><br>

        Displays a vertical list of links to top-level pages. Use when:
        <ul>
        <li>The pages exist at the top level.</li>
        <li>There are many navigational items (more than 5).</li>
        <li>You don't expect users to switch between pages frequently.</li>
        </ul>
:::列結束:::

::: 列:::::: 欄:::![主要詳細資料影像](images/nav/thumbnail-master-detail.svg)::: 欄結束:::::: 欄範圍="2"::: <a href="../controls-and-patterns/master-details.md"><b>主要/詳細資料</b></a><br>

        Displays a list (master view) of items. Selecting an item displays its corresponding page in the details section. Use when:
        <ul>
        <li>You expect users to switch between child items frequently.</li>
        <li>You want to enable the user to perform high-level operations, such as deleting or sorting, on individual items or groups of items, and also want to enable the user to view or update the details for each item.</li>
        </ul>

        Master/details is well suited for email inboxes, contact lists, and data entry.
:::列結束:::

::: 列:::::: 欄:::![超連結與按鈕影像](images/nav/thumbnail-hyperlinks-buttons.svg)::: 欄結束:::::: 欄範圍="2"::: <a href="../controls-and-patterns/hyperlinks.md"><b>的超連結</b></a>和<a href="../controls-and-patterns/buttons.md"><b>按鈕</b></a><br>

        Embedded navigation elements can appear in a page's content. Unlike other navigation elements, which should be consistent across the pages, content-embedded navigation elements are unique from page to page.
:::列結束:::

## <a name="next-add-navigation-code-to-your-app"></a>後續步驟︰新增瀏覽程式碼到您的 App

下一篇文章：[實作基本瀏覽](navigate-between-two-pages.md)，顯示所需的程式碼，才能使用框架控制項在您的 App 中的兩個頁面之間啟用基本瀏覽。 