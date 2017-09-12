---
author: mijacobs
Description: "在通用 Windows 平台 (UWP) app 中的瀏覽是以瀏覽結構、瀏覽元素和系統層級功能的彈性模型為基礎。"
title: "UWP app 的瀏覽基本知識"
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: a944e02ea116c0e054918397d9d46d366d43622a
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2017
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP app 的瀏覽設計基本知識

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

如果您將 App 想成是頁面的集合，則*瀏覽*一詞描述的是頁面之間和頁面內的移動動作。 它是使用者體驗的起點。 是使用者如何尋找他們所感興趣的內容和功能的方式。 它非常重要，也可能難以正確開始。 

> **重要 API**：[Frame](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame)、[Pivot 類別](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Pivot)、[NavigationView 類別](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

難以正確的部分原因是，身應用程式設計，我們要面對許許多多的選擇。 假如我們設計書籍，我們的選擇很簡單︰章節的排列順序。 對於 App，我們可以建立模擬書籍的瀏覽體驗，要求使用者依序瀏覽一系列的頁面。 或者，我們可以提供功能表，讓使用者直接跳到所要的任何頁面，但若我們有太多頁面，我們可能會讓使用者淹沒在選擇當中。 或者，我們可以將所有東西放在單一頁面上，並提供檢視內容的篩選機制。 

當沒有單一的瀏覽設計可適用於每一個 App 時，倒是有一組原則和指導方針您可以遵循，幫助您想出適合您的 App 的設計。 

## <a name="principles-of-good-design"></a>良好設計的原則 
讓我們先從基本原則開始，研究已顯示良好的瀏覽設計的基礎︰ 

* 保持一致︰符合使用者的期望。
* 保持簡單︰不超過您的需求。
* 保持初始狀態︰不要讓瀏覽功能成為使用者的絆腳石。

### <a name="be-consistent"></a>保持一致 
瀏覽應該與使用者的預期一致，仰賴圖示、位置和樣式的標準慣例。 

例如，在以下圖例中，您可以看到使用者通常預期的位置，以尋找功能，例如瀏覽窗格和命令列。 不同裝置系列有其自己的瀏覽元素慣例。 例如，瀏覽窗格通常會出現在平板電腦螢幕的左側，而行動裝置則是上方。

<figure class="wdg-figure">
  ![瀏覽元素的慣用位置](images/nav/nav-component-layout.png)
  <figcaption>使用者預期會在標準位置尋找特定 UI 元素。</figcaption>
</figure> 

### <a name="keep-it-simple"></a>保持簡單
瀏覽設計的另一個重要要素是希克海曼定律，通常在與瀏覽選項相關時引用。 此定律鼓勵我們新增較少的選項到功能表中。 選項越多，使用者與它們互動時的速度會越慢，尤其是使用者在探索新的 App 時。 

<figure class="wdg-figure">
  ![簡單與複雜的功能表](images/nav/nav-simple-menus.png)
  <figcaption> 請注意左側可讓使用者選取的選項較少，而右側則有好幾個。 希克海曼定律指出，左側功能表讓使用者比較容易了解和使用。
</figcaption>
</figure> 

### <a name="keep-it-clean"></a>保持初始狀態
瀏覽的最後一個主要特徵是單純的互動，指的是使用者在各種內容中與瀏覽互動的實際方式。 這是一個您可為使用者設身處地的區域，並會告知您的設計。 請嘗試了解您的使用者及其行為。 例如，如果您要設計烹飪的 App，可以考慮讓您輕鬆存取購物清單及計時器。 

## <a name="three-general-rules"></a>三個一般規則
現在，來看看我們的設計原則 - 一致性、簡化和單純互動 - 並使用它們來想出一些一般規則。 如同任何經驗法則，先將其用作起點，並視需要進行調整。 

1. 避免深層的瀏覽階層。 多少導覽層級最適合您的使用者？ 最上層瀏覽和下方一層就很多了。 如果超出三個層級的瀏覽，就打破簡化的原則。 更糟糕的是，您會有讓您的使用者陷在深度階層中而難以離開。

2. 避免太多瀏覽選項。 每個層級三到六個瀏覽元素最常見。 如果您的瀏覽需求超過這個，尤其是在您的階層的頂層，則您可能要考慮將您的 App 分割為多個 App，因為您可能正嘗試在一個地方執行太多項目。 App 中太多瀏覽元素通常會導致不一致及不相關的目標。

3. 避免「上下彈跳」。 當有相關的內容時，但瀏覽到該內容卻要求使用者往上一層然後再往下一層，就發生上下彈跳狀況。 彈簧單高蹺違反單純互動的原則，要求不必要的點按或互動來達到明顯的目標，在此狀況中，看看一系列的相關內容  (此規則的例外是在搜尋及瀏覽中，上下彈跳可能是唯一提供多樣性和深度所需的方式)。
<figure class="wdg-figure">
  ![上下彈跳的一個範例](images/nav/nav-pogo-sticking-1.png)
  <figcaption> 上下彈跳以瀏覽 App 時，為了瀏覽到 \[專案\] 索引標籤，使用者必須返回 (綠色返回箭號) 主頁面。
</figcaption>
</figure> 
<figure class="wdg-figure">
  ![透過撥動側面瀏覽修正上下彈跳的問題](images/nav/nav-pogo-sticking-2.png)
  <figcaption>您可以使用圖示解析一些 上下彈跳問題 (請注意綠色的撥動手勢)。
</figcaption>
</figure> 


## <a name="use-the-right-structure"></a>使用適當的結構 
現在您已熟悉一般瀏覽原則和規則，可以開始進行一些最重要的瀏覽決策︰如何應該組織您的 App 結構？ 有兩個一般結構︰單層式和階層式。 

### <a name="flatlateral"></a>單層式/側面
在單層式/側面結構中，頁面是並排存在。 您可以任何順序來回在頁面間瀏覽。 
<figure class="wdg-figure">
  <img src="images/nav/nav-pages-peer.png" alt="Pages arranged in a flat structure" />
<figcaption>單層式結構排列的頁面</figcaption>
</figure> 

單層式結構有許多好處︰簡單且容易了解，並且可讓使用者直接跳到特定頁面而不用費力地經過中繼頁面。  一般而言，單層式結構不錯，但並不適用於每一個 App。 如果您的 App 有很多網頁，一般清單可能會變得非常龐大。 如果需要以特定順序檢視頁面，單層式結構就不適用。 

我們建議使用單層式結構的狀況是︰ 
<ul>
<li>頁面可以用任何順序瀏覽。</li>
<li>頁面之間有清楚的區別，且沒有明顯的父系/子系關係。</li>
<li>群組中頁面的數量少於 8 個。<br/>
當群組中的頁面數超過 7 個時，使用者可能會難以了解頁面的獨特性，或難以了解他們目前在群組中的位置。 如果您認為這對您的應用程式不構成問題，那請將頁面以對等方式排列。 否則，請考慮使用階層結構將頁面分成兩組，或分成多個較小的群組。 (中樞控制項可以協助您將群組頁面分類)。</li>
</ul>


### <a name="hierarchical"></a>階層式

在階層式結構中，頁面是組織成類似樹狀的結構。 每個子頁面都只有一個父頁面，但父頁面可以有一或多個子頁面。 若要進入子頁面，您必須經過父頁面。

<figure class="wdg-figure">
  <img src="images/nav/nav-pages-hiearchy.png" alt="Pages arranged in a hierarchy" />
<figcaption>階層排列的頁面</figcaption>
</figure>

階層式結構是組織跨很多頁面的複雜內容或應以特定順序檢視頁面的很好結構。 缺點是階層式頁面會增加一些瀏覽負荷︰較深層的結構、使用者需要多按幾下才能在頁面間瀏覽。 

我們建議使用階層式結構的狀況是︰ 
<ul>
<li>您希望使用者以特定順序周遊頁面。 按階層排列以強制使用該順序。</li>
<li>群組的其中一個頁面與其他頁面之間有清楚的父系與子系關係。</li>
<li>群組中的頁面數超過 7 個。<br/>
當群組中的頁面數超過 7 個時，使用者可能會難以了解頁面的獨特性，或難以了解他們目前在群組中的位置。 如果您認為這對您的應用程式不構成問題，那請將頁面以對等方式排列。 否則，請考慮使用階層結構將頁面分成兩組，或分成多個較小的群組。 (中樞控制項可以協助您將群組頁面分類)。</li>
</ul>

### <a name="combining-structures"></a>結合結構
您沒有選擇一個結構或其他結構；許多妥善設計的 App 使用單層式結構和階層式結構二者︰

![具備混合式結構的 App](images/nav/nav-hybridstructure.png.png)

這些 App 會在最上層頁面使用單層式結構，可以任何順序檢視頁面，而具有較複雜關係的頁面則採用階層式結構。 

如果您的瀏覽結構有多個層級，我們建議讓對等瀏覽元素只連結到其目前所在子樹系中的對等元素。 請思考下列圖例，其中顯示具有三個層級的瀏覽結構：

![具有兩個子樹系的 app](images/nav/nav-subtrees.png)
-   對於層級 1，對等瀏覽元素應提供對頁面 A、B、C 和 D 的存取功能。
-   在層級 2 中，A2 頁面的對等瀏覽元素應只連結到其他 A2 頁面。 它們不應連結到 C 子樹系中的層級 2 頁面。

![具有兩個子樹系的 App](images/nav/nav-subtrees2.png)
 

## <a name="use-the-right-controls"></a>使用適當的控制項

一旦您決定頁面結構之後，您必須決定使用者瀏覽頁面的方式。 UWP 提供各種不同的瀏覽控制項可協助您。 因為每個 UWP app 都可以使用這些控制項，使用它們有助於確保一致且可靠的瀏覽體驗。 


<table>
<tr>
    <th>控制項</th>
    <th>說明</th>
</tr>
<tr>
    <td>[框架](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame)</td>
    <td>有幾個例外，即任何有多個頁面使用框架的 App。 在一般設定中，App 會有主要頁面，包含框架和主要瀏覽元素，例如瀏覽檢視控制項。 當使用者選取頁面時，框架會載入並且顯示。</td>
</tr>
<tr>
    <td>[索引標籤和樞紐](../controls-and-patterns/tabs-pivot.md)<br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>顯示在相同層級之頁面的連結清單。
<p>使用索引標籤/樞紐的時機：</p>
<ul>
<li><p>有 2 到 5 個頁面。</p>
<p>(當超過 5 個頁面時您可以使用索引標籤/樞紐，但可能難以在螢幕上顯示所有的索引標籤/樞紐。)</p></li>
<li>您預期使用者會在頁面之間頻繁切換。</li>
</ul></td>
</tr>
<tr>
    <td>[導覽檢視](../controls-and-patterns/navigationview.md)<br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>顯示連結到最上層頁面的連結清單。
<p>使用瀏覽窗格的時機：</p>
<ul>
<li>您預期使用者不會在頁面之間頻繁切換。</li>
<li>您想要以較慢的瀏覽操作換取空間。</li>
<li>頁面位於最上層。</li>
</ul></td>
</tr>
<tr>
<td>[主要/詳細資料](../controls-and-patterns/master-details.md)<br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>顯示項目摘要的清單 (主要檢視)。 選取在詳細區段中顯示其相對應項目頁面的項目。
<p>使用主要/詳細資料元素的時機：</p>
<ul>
<li>您預期使用者會在子項目之間頻繁切換。</li>
<li>您想讓使用者對個別項目或項目群組執行高層級操作 (例如刪除或排序)，並且也想讓使用者能夠檢視或更新每個項目的詳細資料。</li>
</ul>
<p>主要/詳細資料元素適合電子郵件收件匣、連絡人清單及資料項目。</p>
</td>
</tr>
<tr>
<td s>[返回](navigation-history-and-backwards-navigation.md)</td>
<td style="vertical-align:top;">讓使用者周遊 app 內及 app 之間 (視裝置而定) 的瀏覽歷程記錄。 如需詳細資訊，請參閱[瀏覽歷程記錄和向後瀏覽文章](navigation-history-and-backwards-navigation.md)。</td>
</tr>
<tr class="odd">
<td>超連結和按鈕</td>
<td>內嵌於內容的瀏覽元素會顯示在頁面的內容中。 不同於其他瀏覽元素在頁面群組或子樹系間皆須保持一致，內嵌於內容的瀏覽元素會隨頁面不同而改變。</td>
</tr>
</table>

## <a name="next-add-navigation-code-to-your-app"></a>後續步驟︰新增瀏覽程式碼到您的 App
下一篇文章：[實作基本瀏覽](navigate-between-two-pages.md)，顯示所需的 XAML 和程式碼，才能使用框架控制項在您的 App 中啟用基本瀏覽。 


<!--
## History and the back button
UWP provides a back button and a system for traversing the user's navigation hsitory within an app. This system does most of the work for you, but there are a few APIs you need to call so that it works properly. For more info and instructions, see [History and backwards navigation](navigation-history-and-backwards-navigation.md). 
-->





