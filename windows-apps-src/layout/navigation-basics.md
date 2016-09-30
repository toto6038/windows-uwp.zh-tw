---
author: mijacobs
Description: "在通用 Windows 平台 (UWP) app 中的瀏覽是以瀏覽結構、 瀏覽元素和系統層級功能的彈性模型為基礎。"
title: "通用 Windows 平台 (UWP) app 的瀏覽設計基本知識"
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 6926d70c7140b1545a8b5492981d6d0b61af3784

---

#  UWP app 的瀏覽設計基本知識

在通用 Windows 平台 (UWP) app 中的瀏覽是以瀏覽結構、 瀏覽元素和系統層級功能的彈性模型為基礎。 其相互搭配讓使用者在往來移動於 app、頁面和內容之間時享有直覺式的使用者體驗。

在某些情況下，您或許能夠將 app 的所有內容與功能容納於單一頁面上，讓使用者不需執行任何平移以外的動作即可瀏覽完整內容。 不過，大部分的應用程式通常會有多個頁面的內容與功能需要探索和互動。 當應用程式有一個以上的頁面時，您需要提供的適當的瀏覽體驗。

以使用者的需求為考量，UWP app 中的多頁瀏覽經驗的使用者包括 (在稍後詳細說明)：

-   **適當的瀏覽結構**

    建立使用者可理解的瀏覽結構對於建立直覺式的瀏覽體驗而言非常重要。

-   支援所選結構的**相容瀏覽元素**。

    瀏覽元素可協助使用者取得想要的內容，而且可以讓使用者知道他們在 app 的哪個部分。 不過，它們也會佔用內容或命令元素可用的空間，因此，使用適合您應用程式結構的瀏覽元素非常重要。

-   **能夠適當回應系統層級的瀏覽功能 (如返回)。**

    若要提供直覺化且一致的體驗，建議您依使用者可預期的方式提供系統層級瀏覽功能。

## <span id="Build_the_right_navigation_structure"></span><span id="build_the_right_navigation_structure"></span><span id="BUILD_THE_RIGHT_NAVIGATION_STRUCTURE"></span>建立適當的瀏覽結構


讓我們把應用程式看成許多組頁面的集合，每個頁面都包含一組獨特的內容或功能。 例如，相片應用程式有拍攝相片的頁面、編輯影像的頁面，及管理影像庫的頁面。 您將這些頁面分組的方式會決定應用程式的瀏覽結構。 安排一組頁面的常見方式有兩種：

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">階層關係</th>
<th align="left">對等關係</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><img src="images/nav/nav-pages-hiearchy.png" alt="Pages arranged in a hierarchy" /></p></td>
<td align="left"><p><img src="images/nav/nav-pages-peer.png" alt="Pages arranged as peers" /></p></td>
</tr>
<tr class="even">
<td align="left"><p>頁面已組織成樹狀結構。 每個子頁面都只有一個父頁面，但父頁面可以有一或多個子頁面。 若要進入子頁面，您必須經過父頁面。</p></td>
<td align="left"><p>頁面以並列的方式存在。 您可以依任何順序從一個頁面瀏覽到另一個。</p></td>
</tr>
</tbody>
</table>

 

典型的 app 兩種排列方式都會使用，將某些部分以對等方式排列，而某些部分以階層方式排列。

![具備混合式結構的 app](images/nav/nav-hybridstructure.png.png)

何時該把頁面以階層方式排列，何時又該以對等方式排列呢？ 若要回答此問題，我們必須考慮群組中頁面的數量、頁面是否需以特定順序周遊，以及頁面之間的關係。 一般而言，平面化結構較容易了解且瀏覽速度較快，但有時分層的結構會比較適當。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>我們建議使用階層關係的時機</p>
<ul>
<li><p>您希望使用者以特定順序周遊頁面。 按階層排列以強制使用該順序。</p></li>
<li><p>群組的其中一個頁面與其他頁面之間有清楚的父系與子系關係。</p></li>
<li><p>如果群組中的頁面數超過 7 個。</p>
<p>當群組中的頁面數超過 7 個時，使用者可能會難以了解頁面的獨特性，或難以了解他們目前在群組中的位置。 如果您認為這對您的應用程式不構成問題，那請將頁面以對等方式排列。 否則，請考慮使用階層結構將頁面分成兩組，或分成多個較小的群組。 (中樞控制項可以協助您將群組頁面分類。)</p></li>
</ul></td>
<td align="left"><p>我們建議使用對等關係的時機</p>
<ul>
<li>頁面可以用任何順序瀏覽。</li>
<li>頁面之間有清楚的區別，且沒有明顯的父系/子系關係。</li>
<li><p>群組中頁面的數量少於 8 個。</p>
<p>當群組中的頁面數超過 7 個時，使用者可能會難以了解頁面的獨特性，或難以了解他們目前在群組中的位置。 如果您認為這對您的應用程式不構成問題，那請將頁面以對等方式排列。 否則，請考慮使用階層結構將頁面分成兩組，或分成多個較小的群組。 (中樞控制項可以協助您將群組頁面分類。)</p></li>
</ul></td>
</tr>
</tbody>
</table>

 

## <span id="Use_the_right_navigation_elements"></span><span id="use_the_right_navigation_elements"></span><span id="USE_THE_RIGHT_NAVIGATION_ELEMENTS"></span>使用正確的瀏覽元素


瀏覽元素可以提供兩種服務：它們能協助使用者取得想要的內容，而且某些元素可以讓使用者知道他們在應用程式的哪個部分。 不過，它們也會佔用應用程式用於內容或命令元素的空間，因此，使用適合您應用程式結構的瀏覽元素非常重要。

### <span id="Peer-to-peer_navigation_elements"></span><span id="peer-to-peer_navigation_elements"></span><span id="PEER-TO-PEER_NAVIGATION_ELEMENTS"></span>對等瀏覽元素

對等瀏覽元素可在同子樹系且同層級的頁面之間瀏覽。

![對等瀏覽](images/nav/nav-lateralmovement.png)

針對對等瀏覽，我們建議使用索引標籤或瀏覽窗格。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">瀏覽元素</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[索引標籤和樞紐](../controls-and-patterns/tabs-pivot.md)</p>
<p><img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></p></td>
<td align="left">顯示相同層級中持續不變的頁面連結清單。
<p>使用索引標籤/樞紐的時機：</p>
<ul>
<li><p>有 2 到 5 個頁面。</p>
<p>(當超過 5 個頁面時您可以使用索引標籤/樞紐，但可能難以在螢幕上顯示所有的索引標籤/樞紐。)</p></li>
<li>您預期使用者會在頁面之間頻繁切換。</li>
</ul>
<p>尋找餐廳 app 的這個設計使用索引標籤/樞紐：</p>
<p><img src="images/food-truck-finder/uap-foodtruck-tabletphone-sbs-sm-400.png" alt="Example of an app using tabs/pivots pattern" /></p></td>
</tr>
<tr class="even">
<td align="left"><p>[瀏覽窗格](../controls-and-patterns/nav-pane.md)</p>
<p><img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></p></td>
<td align="left">顯示連結到最上層頁面的連結清單。
<p>使用瀏覽窗格的時機：</p>
<ul>
<li>您預期使用者不會在頁面之間頻繁切換。</li>
<li>您想要以較慢的瀏覽操作換取空間。</li>
<li>頁面位於最上層。</li>
</ul>
<p>這個智慧型住宅 app 的設計強調瀏覽窗格：</p>
<p><img src="images/smart-home/uap-smarthome-tabletphone-sbs-sm-400.png" alt="Example of an app that uses a nav pane pattern" /></p>
<p></p></td>
</tr>
</tbody>
</table>

 

如果您的瀏覽結構有多個層級，我們建議讓對等瀏覽元素只連結到其目前所在子樹系中的對等元素。 請考慮下列圖例，其中顯示具有三個層級的瀏覽結構：

![具有兩個子樹系的 app](images/nav/nav-subtrees.png)
-   對於層級 1，對等瀏覽元素應提供對頁面 A、B、C 和 D 的存取功能。
-   在層級 2 中，A2 頁面的對等瀏覽元素應只連結到其他 A2 頁面。 它們不應連結到 C 子樹系中的層級 2 頁面。

![具有兩個子樹系的 app](images/nav/nav-subtrees2.png)

### <span id="Hierarchical_navigation_elements"></span><span id="hierarchical_navigation_elements"></span><span id="HIERARCHICAL_NAVIGATION_ELEMENTS"></span>階層瀏覽元素

階層瀏覽元素可提供在父頁面與其子頁面之間的瀏覽功能。

![階層瀏覽](images/nav/nav-verticalmovement.png)

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">瀏覽元素</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[中樞](../controls-and-patterns/hub.md)</p>
<p><img src="images/higsecone-hub-thumb.png" alt="Hub" /></p></td>
<td align="left">中樞是一種特殊類型的瀏覽控制項，可提供其子頁面的預覽/摘要。 不同於瀏覽窗格或索引標籤，它會透過頁面本身內嵌的連結和區段標題，提供對這些子頁面的瀏覽功能。
<p>使用中樞的時機：</p>
<ul>
<li>您預期使用者會想要在不需要瀏覽每個子頁面的情況下檢視子頁面的部分內容。</li>
</ul>
<p>中樞可以用來探索，所以它們適合用於媒體、新聞閱讀程式，以及購物應用程式。</p>
<p></p></td>
</tr>
<tr class="even">
<td align="left"><p>[主要/詳細資料](../controls-and-patterns/master-details.md)</p>
<p><img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></p></td>
<td align="left">顯示項目摘要的清單 (主要檢視)。 選取在詳細區段中顯示其相對應項目頁面的項目。
<p>使用主要/詳細資料元素的時機：</p>
<ul>
<li>您預期使用者會在子項目之間頻繁切換。</li>
<li>您想讓使用者對個別項目或項目群組執行高層級操作 (例如刪除或排序)，並且也想讓使用者能夠檢視或更新每個項目的詳細資料。</li>
</ul>
<p>主要/詳細資料元素適合電子郵件收件匣、連絡人清單及資料項目。</p>
<p>這個股票追蹤 app 的設計採用主要/詳細資料模式：</p>
<p><img src="images/stock-tracker/uap-finance-tabletphone-sbs-sm.png" alt="Example of a stock trading app that has a master/details pattern" /></p></td>
</tr>
</tbody>
</table>

 

### <span id="Historical_navigation_elements"></span><span id="historical_navigation_elements"></span><span id="HISTORICAL_NAVIGATION_ELEMENTS"></span>歷程瀏覽元素

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">瀏覽元素</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">返回</td>
<td align="left"><p>讓使用者周遊 app 內及 app 之間 (視裝置而定) 的瀏覽歷程記錄。 如需詳細資訊，請參閱本文章稍後的[讓您的 app 能搭配系統層級的瀏覽功能運作](#backnavigation)。</p></td>
</tr>
</tbody>
</table>

 

### <span id="Content-embedded_navigation_elements"></span><span id="content-embedded_navigation_elements"></span><span id="CONTENT-EMBEDDED_NAVIGATION_ELEMENTS"></span>內嵌於內容的瀏覽元素

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">瀏覽元素</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">超連結和按鈕</td>
<td align="left"><p>內嵌於內容的瀏覽元素會顯示在頁面的內容中。 不同於其他瀏覽元素在頁面群組或子樹系間皆須保持一致，內嵌於內容的瀏覽元素會隨頁面不同而改變。</p></td>
</tr>
</tbody>
</table>

 

### <span id="Combining_navigation_elements"></span><span id="combining_navigation_elements"></span><span id="COMBINING_NAVIGATION_ELEMENTS"></span>結合瀏覽元素

您可以結合瀏覽元素以建立最適合您的 app 的瀏覽體驗。 例如，您的 app 可能會使用瀏覽窗格以提供最上層頁面的存取，使用索引標籤以提供第二層頁面的存取。






 







<!--HONumber=Jun16_HO4-->


