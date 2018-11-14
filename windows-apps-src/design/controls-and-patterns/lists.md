---
author: muhsinking
Description: Lists display and enable interaction with collection-based content.
title: 清單
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Lists
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 3594697292d6dfe9435036b838a0ba4bd2dbfc05
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6465537"
---
# <a name="lists"></a>清單

會顯示清單，並啟用與集合內容的互動。 本文中涵蓋的四個清單模式包括：

-   清單檢視，主要用來顯示大量文字內容集合
-   資料格檢視，主要用來顯示大量影像內容集合
-   下拉式清單，讓使用者從展開的清單中選擇一個項目
-   清單方塊，讓使用者從可以捲動的方塊中選擇一或多個項目

針對每個清單模式指定設計指導方針、功能和範例。

> **重要 API**：[ListView 類別](https://msdn.microsoft.com/library/windows/apps/br242878)、[GridView 類別](https://msdn.microsoft.com/library/windows/apps/br242705)、[ComboBox 類別](https://msdn.microsoft.com/library/windows/apps/br209348)


> <div id="main">
> <strong>Windows 10 Fall Creators Update - 行為變更</strong>
> </div>
> 根據預設，主動式手寫筆現在可在 UWP app 中捲動/移動瀏覽 (例如觸控、觸控板和被動式手寫筆)，而不用執行選擇。
> 如果您的應用程式取決於先前的行為，您可以覆寫手寫筆捲動並還原回先前的行為。 請參閱 [ [ScrollViewer 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer)API 參考主題，如需詳細資訊。

## <a name="list-views"></a>清單檢視

清單檢視可讓您將項目分類並指派群組標頭、拖放項目、規劃內容，以及重新排序項目。

### <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用清單檢視：

-   顯示主要由文字所組成的內容集合。
-   瀏覽的內容為單一或已分類的集合。
-   在[主要/詳細資料模式](master-details.md)中建立主要窗格。 主要/詳細資料是經常用於電子郵件應用程式的模式，其中一個窗格 (主要) 具有可選取項目的清單，而另一個窗格 (詳細資料) 具有已選取項目的詳細資料檢視。

### <a name="examples"></a>範例

以下是在手機上顯示分組資料的簡單清單檢視。

![具有分組資料的清單檢視](images/simple-list-view-phone.png)

### <a name="recommendations"></a>建議事項

-   清單中的項目應該有相同的行為。
-   如果您的清單會分成群組，您可以使用[語意式縮放](semantic-zoom.md)，這樣能讓使用者更容易瀏覽已分組的內容。

### <a name="list-view-articles"></a>清單檢視文章
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">清單檢視和方格檢視</a></p></td>
<td align="left"><p>了解在應用程式中使用清單檢視或方格檢視的基本資訊。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">項目容器與範本</a></p></td>
<td align="left"><p>您以清單或方格顯示的項目，將能在應用程式整體外觀上扮演重要的角色。 修改控制項範本和資料範本以定義項目的外觀，並讓您的應用程式看起來更美觀。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">清單檢視的項目範本</a></p></td>
<td align="left"><p>使用 ListView 的這些範例項目範本可取得通用應用程式類型的外觀。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">反轉清單</a></p></td>
<td align="left"><p>反轉清單會從底部開始加入新項目，例如聊天應用程式。 遵循此指導方針以在應用程式中使用反轉清單。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">拖動以重新整理</a></p></td>
<td align="left"><p>拖動以重新整理模式可讓使用者以觸控的方式將資料清單向下拖動以抓取更多資料。 使用此指導方針以在清單檢視中實作拖動重新整理。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">巢狀 UI</a></p></td>
<td align="left"><p>巢狀 UI 是一種使用者介面 (UI)，能夠公開包含在容器中的可動作控制項，供使用者採取動作。 例如，您有一個包含按鈕的清單檢視項目，而使用者可以選取該清單項目，或按下巢嵌在其中的按鈕。 請遵循這些最佳做法來為使用者提供最佳的巢狀 UI 體驗。</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>方格檢視

資料格檢視適合用來排列和瀏覽以影像為基礎的內容集合。 資料格檢視版面配置垂直捲動和水平移動瀏覽。 項目會以從左至右，然後從上至下的閱讀順序進行配置。

### <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用清單檢視：

-   顯示主要由影像所組成的內容集合。
-   顯示內容庫。
-   將與[語意式縮放](semantic-zoom.md)關聯的兩個內容檢視格式化。

### <a name="examples"></a>範例

這個範例顯示典型的資料格檢視版面配置 ，在此案例中是針對瀏覽應用程式。 資料格檢視項目的中繼資料通常受限於幾行文字與項目評等。

![資料格檢視配置範例](images/controls_gridview_example02.png)

資料格檢視是內容庫的理想解決方案，通常用來呈現如圖片和視訊等等的媒體。 在內容庫中，使用者預期能夠點選項目來叫用動作。

![內容庫範例](images/controls_list_contentlibrary.png)

### <a name="recommendations"></a>建議

-   清單中的項目應該有相同的行為。
-   如果您的清單會分成群組，您可以使用[語意式縮放](semantic-zoom.md)，這樣能讓使用者更容易瀏覽已分組的內容。

### <a name="grid-view-articles"></a>方格檢視文章
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">清單檢視和方格檢視</a></p></td>
<td align="left"><p>了解在應用程式中使用清單檢視或方格檢視的基本資訊。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">項目容器與範本</a></p></td>
<td align="left"><p>您以清單或方格顯示的項目，將能在應用程式整體外觀上扮演重要的角色。 修改控制項範本和資料範本以定義項目的外觀，並讓您的應用程式看起來更美觀。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">方格檢視的項目範本</a></p></td>
<td align="left"><p>使用 GridView 的這些範例項目範本可取得通用應用程式類型的外觀。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">巢狀 UI</a></p></td>
<td align="left"><p>巢狀 UI 是一種使用者介面 (UI)，能夠公開包含在容器中的可動作控制項，供使用者採取動作。 例如，您有一個包含按鈕的清單檢視項目，而使用者可以選取該清單項目，或按下巢嵌在其中的按鈕。 請遵循這些最佳做法來為使用者提供最佳的巢狀 UI 體驗。</p></td>
</tr>
</tbody>
</table>

## <a name="drop-down-lists"></a>下拉式清單

下拉式清單 (也稱為下拉式方塊) 一開始為精簡狀態，展開後會顯示可選取項目清單。 已選取的項目一律為可見狀態，而不可見的項目則是會在使用者點選下拉式方塊來展開時顯示。

### <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

-   若要讓使用者從能夠以單行文字充分表示的一組項目中選取單一值，請使用下拉式清單。
-   若要顯示包含多行文字或影像的項目，請使用清單或資料格檢視，而不要使用下拉式清單。
-   如果項目少於五個，請考慮使用[選項按鈕](radio-button.md) (如果是單選) 或[核取方塊](checkbox.md) (如果是複選)。
-   如果選項項目在您應用程式流程中的重要性為次要，請使用下拉式方塊。 如果針對大部分使用者在大多數情況下建議使用預設選項，則使用清單方塊來顯示所有項目可能會讓使用者浪費過多注意力在選項上。 您可以藉由使用下拉式方塊來節省空間，以及減少注意力分散的情形。

### <a name="examples"></a>範例

精簡狀態下的下拉式清單可以顯示標頭。

![精簡狀態下的下拉式清單範例](images/combo_box_collapsed.png)

雖然下拉式方塊可延展來支援較長的字串長度，但是請避免使用過長而難以閱讀的字串。

![具有長文字字串的下拉式清單範例](images/combo_box_listitemstate.png)

如果下拉式方塊中的集合夠長，將會顯示捲軸以容納它。 將清單中的項目以邏輯方式分組。

![下拉式清單中捲軸的範例](images/combo_box_scroll.png)

### <a name="recommendations"></a>建議事項

-   將下拉式方塊項目的文字內容限制為單行。
-   以最合乎邏輯的順序排序下拉式方塊中的項目。 將相關選項群組在一起並將最常用的選項放在頂端。 以字母順序排序名稱、以數字順序排序數字，以及以時間順序排序日期。
-   若要建立會在使用者使用方向鍵 (例如 [字型選擇] 下拉式清單) 時即時更新的下拉式方塊，請將 SelectionChangedTrigger 設定為 [永遠]。  

### <a name="text-search"></a>文字搜尋

下拉式方塊可自動支援在其集合內的搜尋。 當焦點在一個已開啟或關閉的下拉式方塊上時，如果使用者在實體鍵盤上輸入字元，就會顯示與使用者的字串相符的候選項目。 在瀏覽長清單時，這項功能特別有幫助。 例如，與包含狀態清單的下拉式清單進行互動時，使用者可以按 “w” 鍵來顯示 “Washington” 以供快速選取。


## <a name="list-boxes"></a>清單方塊

清單方塊可讓使用者從集合中選擇一個項目或多個項目。 清單方塊類似下拉式清單，不同的是清單方塊一律會開啟；清單方塊沒有精簡 (非展開) 狀態。 如果無法一次顯示所有項目，則可以捲動清單中的項目。

### <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

-   當清單中的項目很重要而必須特別顯示，以及當有足夠的螢幕實際可用空間可以顯示完整的清單時，清單方塊很有用。
-   清單方塊應該讓使用者在進行重大選擇時，能夠注意到整組替代項目。 相反地，下拉式清單一開始就會吸引使用者對於選取的項目的注意。
-   如果有下列情形，則避免使用清單方塊：
    -   清單中只有非常少量的項目。 總是有相同的 2 個選項的單選清單方塊，最好以[選項按鈕](radio-button.md)的形式呈現。 清單中有 3 個或 4 個靜態項目時，也可以考慮使用選項按鈕。
    -   清單方塊屬於單選形式，且總是有相同的 2 個選項暗示彼此互異，例如「開」與「關」。 請使用單一核取方塊或切換開關。
    -   有非常大量的項目。 針對較長的清單最好選擇資料格檢視與清單檢視。 針對清單非常冗長的分組資料，最好使用語意式縮放。
    -   項目為連續數值。 如果是這種情況，請考慮[滑桿](slider.md)。
    -   對於大部分案例中的多數使用者，建議選取項目為使用應用程式流程中重要性居次的項目或預設選項。 請改用下拉式清單。

### <a name="recommendations"></a>建議

-   清單方塊中理想的項目範圍是 3 到 9 個。
-   即使其中的項目大不相同，清單方塊仍能正常運作。
-   盡可能將清單方塊的大小設定為不需要移動瀏覽或捲動項目清單。
-   確認清單方塊的用途和目前選取的項目顯而易見。
-   保留觸控回饋和已選取項目狀態的視覺化效果與動畫。
-   清單方塊項目的文字內容以一行為限。 如果項目是視覺元素，您可以自訂項目的大小。 如果項目包含多行文字或影像，請改用資料格檢視或清單檢視。
-   除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。
-   不要使用清單方塊執行命令，或動態顯示或隱藏其他控制項。

## <a name="selection-mode"></a>選取模式

選取模式可讓使用者選取一或多個項目，並且採取動作。 它可以透過操作功能表 (在項目上以 CTRL + 按一下或 SHIFT + 按一下) 叫用，或在圖庫檢視中將游標移至項目上的目標。 啟動選取模式時，每個清單項目旁邊都會顯示核取方塊，且在螢幕頂端或底部可能會顯示動作。

有三種選取模式：

-   單一：使用者一次只能選取一個項目。
-   多重：使用者不需要使用輔助按鍵就能選取多個項目。
-   延伸：使用者可以使用輔助按鍵選取多個項目，例如按住 SHIFT 鍵。

在項目上點選任何位置即可選取項目。 點選命令列巨集指令會影響所有選取項目。 如果未選取任何項目，命令列動作應該為非使用中狀態 (除了 [全選] 以外)。

選取模式並沒有消失關閉模型；點選選取模式在使用中之框架的外側並不會取消模式。 這可以避免意外停用模式。 按一下 [上一頁] 按鈕關閉多重選取模式。

在選取巨集指令時顯示視覺化確認。 請考慮針對某些動作顯示確認對話方塊，特別是破壞性動作 (例如刪除)。

選取模式只會影響已啟用該模式的頁面，所以不會影響該頁面以外的任何項目。

選取模式的進入點應該與受其影響的內容並列。

如需命令列的建議，請參閱[命令列的指導方針](app-bars.md)。

## <a name="globalization-and-localization-checklist"></a>全球化和當地語系化檢查清單

<table>
<tr>
<th>換行</th><td>允許兩行的清單標籤。</td>
</tr>
<tr>
<th>水平擴充</th><td>確定欄位可以容納文字擴充，而且可捲動。</td>
</tr>
<tr>
<th>垂直間距</th><td>使用非拉丁字元的垂直間距，確保將適當地顯示非拉丁指令碼。</td>
</tr>
</table>


## <a name="related-articles"></a>相關文章

- [中樞](hub.md)
- [主要/詳細資料](master-details.md)
- [瀏覽窗格](navigationview.md)
- [語意式縮放](semantic-zoom.md)
- [拖放](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [縮圖影像](../../files/thumbnails.md)

**適用於開發人員**
- [ListView 類別](https://msdn.microsoft.com/library/windows/apps/br242878)
- [GridView 類別](https://msdn.microsoft.com/library/windows/apps/br242705)
- [ComboBox 類別](https://msdn.microsoft.com/library/windows/apps/br209348)
- [ListBox 類別](https://msdn.microsoft.com/library/windows/apps/br242868)