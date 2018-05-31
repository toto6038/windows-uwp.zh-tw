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
ms.openlocfilehash: 3edb7dc28561b5d316a908df951e3052eafc995b
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654267"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP app 的瀏覽設計基本知識

如果您將 App 想成是頁面的集合，則*瀏覽*描述的是頁面之間和頁面內的移動動作。 這是使用者體驗的起點，也是使用者尋找感興趣的功能與內容的方式。 它非常重要，也可能難以正確開始。

難的部分在於，身為應用程式設計師，我們要面對許許多多的選擇。 我們可能會要求使用者依序瀏覽一系列頁面。 我們也可能提供功能表，讓使用者直接跳到任何頁面。 或者，我們可以將所有東西放在單一頁面上，並提供檢視內容的篩選機制。
 
當沒有單一的瀏覽設計可適用於每一個 App 時，倒是有原則和指導方針可幫助您決定適合您 App 的設計。 

<figure class="wdg-figure">
  <img alt="Navigation diagram for an app" src="images/navigation_diagram.png" />
  <figcaption>應用程式瀏覽的圖表。</figcaption>
</figure> 

## <a name="principles-of-good-navigation"></a>良好瀏覽的原則
讓我們從良好瀏覽設計的原則開始： 

* 一致性︰符合使用者的期望。
* 簡潔︰不超過您的需求。
* 清楚明確：提供清楚的路徑和選項。

### <a name="consistency"></a>一致性
瀏覽應該與使用者的期望一致。 使用使用者熟悉的[標準控制項](#use-the-right-controls)，並依循圖示、位置和樣式的標準慣例，瀏覽對使用者來說就是可預測且直覺。

<figure class="wdg-figure">
<img src="images/nav/nav-component-layout.png" alt="Preferred location for navigation elements"/>
  <figcaption>使用者預期會在標準位置尋找特定 UI 元素。</figcaption>
</figure> 

### <a name="simplicity"></a>簡潔
瀏覽項目越少，使用者做出決定就越簡單。 讓使用者輕鬆抵達重要的目的地，並隱藏較不重要的項目，可幫助使用者更快到達想要的位置。

<figure class="wdg-figure">
<img src="images/nav/nav-simple-menus.png" alt="A simple versus a complex menu"/>
  <figcaption> 左側功能表讓使用者比較容易了解和使用，因為項目較少。
</figcaption>
</figure> 

### <a name="clarity"></a>清楚明確
清楚的路徑可讓使用者邏輯瀏覽。 提供清楚明瞭的選項，並釐清頁面之間的關係，可防止使用者迷失方向。

<figure class="wdg-figure">
<img src="images/nav/nav-pages.png" alt="Clear paths and destinations"/>
  <figcaption> 將目的地清楚標示出，讓使用者知道自己身在何處。
</figcaption>
</figure> 

## <a name="general-recommendations"></a>一般建議
現在來看看我們的設計原則 - 一致性、簡潔和清楚明確 - 並使用它們來想出一些一般建議。

1. 考量您的使用者。 找出使用者可能在您的 app 中依循的一般途徑，並思考使用者來到每個頁面的原因，以及接著想前往哪裡。 

2. 避免深層的瀏覽階層。 如果您的瀏覽超過三層，可能會讓您的使用者陷在深度階層中而難以離開。

3. 避免「上下彈跳」。 當有相關的內容時，但瀏覽到該內容卻要求使用者往上一層然後再往下一層，就發生上下彈跳狀況。

## <a name="use-the-right-structure"></a>使用適當的結構 
現在您已熟悉一般瀏覽原則，那麼接下來要如何建構您的 app？ 有兩個一般結構︰單層式和階層式。 

### <a name="flatlateral"></a>單層式/側面
![單層式結構排列的頁面](images/nav/nav-pages-peer.png)

在單層式/側面結構中，頁面是並排存在。 您可以任何順序來回在頁面間瀏覽。 

我們建議使用單層式結構的狀況是︰ 
<ul>
<li>頁面可以用任何順序瀏覽。</li>
<li>頁面之間有清楚的區別，且沒有明顯的父系/子系關係。</li>
<li>群組中頁面的數量少於 8 個。<br/>
(有多個頁面時，使用者可能會難以了解頁面的獨特性，或難以了解他們目前在群組中的位置。 如果您認為這對您的應用程式不構成問題，那請將頁面以對等方式排列。 否則，請考慮使用階層結構將頁面分成兩組，或分成多個較小的群組。)</li>
</ul>

### <a name="hierarchical"></a>階層式
![階層排列的頁面](images/nav/nav-pages-hiearchy.png)

在階層式結構中，頁面是組織成類似樹狀的結構。 每個子頁面都有一個父頁面，但父頁面可以有一或多個子頁面。 若要進入子頁面，您必須經過父頁面。

階層結構對於組織跨多個頁面的複雜內容來說很實用。 缺點是會有一些瀏覽負荷︰結構越深層，就需要多按幾下才能在頁面間瀏覽。 

我們建議使用階層式結構的狀況是︰ 
<ul>
<li>頁面應以特定順序周遊。</li>
<li>頁面之間有清楚的父-子關係。</li>
<li>群組中的頁面數超過 7 個。</li>
</ul>

### <a name="combining-structures"></a>結合結構
![具備混合式結構的 App](images/nav/nav-hybridstructure.png.png)

您不是只能選擇一種結構；許多妥善設計的 App 會使用二者。 App 可在最上層頁面使用單層式結構，可以任何順序檢視頁面，而具有較複雜關係的頁面則採用階層式結構。 

如果您的瀏覽結構有多個層級，我們建議讓對等瀏覽元素只連結到其目前所在子樹系中的對等元素。 請思考下列圖例，其中顯示具有三個層級的瀏覽結構：

![具有兩個子樹系的 App](images/nav/nav-subtrees.png)
- 於層級 1，對等瀏覽元素應提供對頁面 A、B、C 和 D 的存取功能。
- 在層級 2 中，A2 頁面的對等瀏覽元素應只連結到其他 A2 頁面。 它們不應連結到 C 子樹系中的層級 2 頁面。

![具有兩個子樹系的 App](images/nav/nav-subtrees2.png)

## <a name="use-the-right-controls"></a>使用適當的控制項
一旦您決定頁面結構之後，您必須決定使用者瀏覽頁面的方式。 UWP 提供各種不同的瀏覽控制項，有助於確保您的 App 提供一致、可靠的瀏覽體驗。 

我們建議您根據 App 中的瀏覽元素數目選取瀏覽控制項。 如果您有五個或更少瀏覽項目，則使用最上層瀏覽，像是[索引標籤和樞紐](../controls-and-patterns/tabs-pivot.md)。 如果您有六個以上的瀏覽項目，則使用左側瀏覽，像是[瀏覽檢視](../controls-and-patterns/navigationview.md)或[主要/詳細資料](../controls-and-patterns/master-details.md)。

<div class="mx-responsive-img">

<table>
<tr>
    <th>控制項</th>
    <th>說明</th>
</tr>
<tr>
    <td><a href="https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame">框架</a><br/><br/>
    <img src="images/frame.png" alt="Frame" /></td>
    <td>顯示頁面。 <p>有幾個例外，即任何有多個頁面的 App 都會使用框架。 一般來說，App 會有主要頁面，包含框架和主要瀏覽元素，例如瀏覽檢視控制項。 當使用者選取頁面時，框架會載入並且顯示。</p></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/tabs-pivot.md">索引標籤和樞紐</a><br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>顯示相同層級中水平的頁面連結清單。
<p>使用時機：</p>
<ul>
<li>有 2 到 5 個頁面。 (當超過 5 個頁面時您可以使用索引標籤/樞紐，但可能難以在螢幕上顯示所有的索引標籤/樞紐。)</li>
<li>您預期使用者會在頁面之間頻繁切換。</li>
</ul></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/navigationview.md">瀏覽檢視</a><br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>顯示連結到最上層頁面的垂直清單。
<p>使用時機：</p>
<ul>
<li>頁面位於最上層。</li>
<li>有許多瀏覽項目 (超過 5 個)。</li>
<li>您預期使用者不會在頁面之間頻繁切換。</li>

</ul></td>
</tr>
<tr>
<td><a href="../controls-and-patterns/master-details.md">主要/詳細資料</a><br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>顯示項目的清單 (主要檢視)。 選取在詳細區段中顯示其相對應頁面的項目。
<p>使用時機：</p>
<ul>
<li>您預期使用者會在子項目之間頻繁切換。</li>
<li>您想讓使用者對個別項目或項目群組執行高層級操作 (例如刪除或排序)，並且也想讓使用者能夠檢視或更新每個項目的詳細資料。</li>
</ul>
<p>主要/詳細資料適合電子郵件收件匣、連絡人清單及資料項目。</p>
</td>
<tr>
<td><a href="../controls-and-patterns/hub.md">中心</a><br/><br/>
<img src="images/hub.png" alt="Hub" /></td>
<td> 在格線中顯示依序排列項目的區段。 
<p>使用時機：</p>
<ul>
<li>您想要建立主題圖片的視覺化瀏覽。</li>
</ul>
<p>中心非常適合用於瀏覽、檢視或購買體驗。</p>
</td>
</tr>
<tr>
<td><a href="../controls-and-patterns/hyperlinks.md">超連結</a>和<a href="../controls-and-patterns/buttons.md">按鈕</a></td>
<td> 內嵌瀏覽元素可顯示在頁面的內容中。 不同於其他瀏覽元素在頁面間皆須保持一致，內嵌於內容的瀏覽元素會隨頁面不同而改變。</td>
</tr>
</table>
</div>

## <a name="next-add-navigation-code-to-your-app"></a>後續步驟︰新增瀏覽程式碼到您的 App
下一篇文章：[實作基本瀏覽](navigate-between-two-pages.md)，顯示所需的程式碼，才能使用框架控制項在您的 App 中的兩個頁面之間啟用基本瀏覽。 