---
description: 在 Windows 應用程式中用於顯示內容的常見頁面模式和 UI 元素的概觀。
title: Windows 應用程式的內容設計基本知識
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4f076911fc1ed4770c9f1d69125c4fc6d0cecdf9
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804864"
---
# <a name="content-design-basics-for-windows-apps"></a>Windows 應用程式的內容設計基本知識

任何一種 app 的主要用途都是讓使用者存取內容。 由於應用程式因多種不同的目的而存在，因此內容會有多種形式：在相片編輯應用程式中，內容是相片；在旅遊應用程式中，內容是地圖與旅遊地點的相關資訊；以此類推。 

本文提供如何在應用程式中呈現內容的概觀。 我們描述常見的頁面模式和 UI 元素，這些可用來顯示您的內容，無論是任何形式。

## <a name="common-page-patterns"></a>常見的頁面模式

許多應用程式會使用這些常見頁面模式的一部分或全部，來顯示不同類型的內容。 同樣地，這些模式可以混搭使用，創造出最佳的應用程式內容。

### <a name="landing"></a>登陸

![登陸頁面](images/content-basics/hero-screen.png)

登陸頁面，也就是主題畫面，通常出現在應用程式體驗的最上層。 大型介面區域做為應用程式的舞台，強調出使用者會想要瀏覽和使用的內容。

### <a name="collections"></a>集合

![圖庫](images/content-basics/gridview.png)

集合可讓使用者瀏覽內容或資料群組。 [方格檢視](../controls-and-patterns/item-templates-gridview.md)是相片或媒體為主內容的理想選擇，而[清單檢視](../controls-and-patterns/item-templates-listview.md)是文字為主內容或資料的理想選擇。


### <a name="listdetail"></a>清單/詳細資料

![清單詳細資料](images/content-basics/list-detail.png)

[清單/詳細資料](../controls-and-patterns/list-details.md)模型是由清單視圖和內容視圖組成 (詳細資料) 。 這兩個窗格都是固定的，並可垂直捲動。 清單專案和內容視圖之間有清楚的關聯性：已選取清單視圖中的專案，而且詳細資料檢視也會更新。 除了提供詳細資料檢視導覽之外，還可以加入和移除清單視圖中的專案。

### <a name="details"></a>詳細資料

![多重檢視](images/multi-view.png)

當使用者尋找想要的內容時，可考慮建立專用的內容檢視頁面，讓使用者能夠專心檢視頁面不受干擾。 可能的話，[建立全螢幕檢視選項](../layout/show-multiple-views.md)，它可以將內容展開到整個螢幕，並隱藏其他的 UI 元素。 

若要隨螢幕大小改變進行調整，同樣可考慮建立[回應式設計](design-and-ui-intro.md)，它可適時隱藏/顯示 UI 元素。

### <a name="forms"></a>表單
![表單](images/content-basics/forms.png)

[表單](../controls-and-patterns/forms.md)是一組控制項，會收集和提交使用者的資料。 大多數 (即使不是全部) 應用程式都會在設定頁面上、登入入口網站、意見反應中樞、帳戶建立或基於其他目的使用某種表單。 

## <a name="common-content-elements"></a>常用內容元素

若要建立這些頁面模式，您將需要使用個別內容元素的組合。 以下是一些常用來顯示內容的 UI 元素。 (如需 UI 元素的完整清單，請參閱[控制項和模式](../controls-and-patterns/index.md)。)

<div class="mx-responsive-img">
<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">類別</th>
<th align="left">元素</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">音訊與視訊<br/><br/>
    <img src="images/content-basics/media-transport.png" alt="media transport control" /></td>
<td align="left"><a href="../controls-and-patterns/media-playback.md">媒體播放和傳輸控制項</a></td>
<td align="left">播放音訊與視訊。</td>
</tr>
<tr class="even">
<td align="left">影像檢視器<br/><br/>
    <img src="images/content-basics/flipview.jpg" alt="flip view" /></td>
<td align="left"><a href="../controls-and-patterns/flipview.md">翻轉檢視</a>、<a href="../controls-and-patterns/images-imagebrushes.md">影像</a></td>
<td align="left">顯示影像。 翻轉檢視顯示集合中的影像 (一次一個影像)，例如相簿中的相片或是產品詳細資料頁面中的項目。</td>
</tr>
<tr class="odd">
<td align="left">集合 <br/><br/>
    <img src="images/content-basics/listview.png" alt="list view" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">清單檢視和方格檢視</a></td>
<td align="left">在互動式清單或方格中呈現項目。 使用這些元素，可讓使用者從最新發行的清單中選取電影，或是管理詳細目錄。</td>
</tr>
<tr class="even">
<td align="left">文字和輸入文字 <br/><br/>
    <img src="images/content-basics/textbox.png" alt="text box" /></td>
<td align="left"><p><a href="../controls-and-patterns/text-block.md">文字區塊</a>、<a href="../controls-and-patterns/text-box.md">文字方塊</a>、<a href="../controls-and-patterns/rich-edit-box.md">可編輯對話方塊</a></p>
</td>
<td align="left">顯示文字。 某些元素可讓使用者編輯文字。 如需詳細資訊，請參閱<a href="../controls-and-patterns/text-controls.md">文字控制項</a>。
<p>如需如何掩飾文字的指導方針，請參閱<a href="../style/typography.md">印刷樣式</a>。</p>
</td>
</tr>
<tr class="odd">
<td align="left">地圖<br/><br/>
    <img src="images/content-basics/mapcontrol.png" alt="map control" /></td>
<td align="left"><a href="../../maps-and-location/display-maps.md">MapControl</a></td>
<td align="left">顯示地球的象徵性或真實感地圖。</td>
</tr>
<tr class="even">
<td align="left">WebView</td>
<td align="left"><a href="../controls-and-patterns/web-view.md">WebView</a></td>
<td align="left">呈現 HTML 內容。</td>
</tr>
</tbody>
</table>
</div>
