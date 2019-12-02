---
Description: 會顯示清單，並啟用與集合內容的互動。
title: 集合和清單
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Collections and Lists
template: detail.hbs
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 06cd49ce68de6f1c7a7a29b94c80f0a004a2eca3
ms.sourcegitcommit: 6b29f0cbdc6e66b44150b3b60e95d67e1f7f56bf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74478535"
---
# <a name="collections-and-lists"></a>集合和清單

集合和清單都可用來表示會一起出現的多個相關資料項目。 集合可透過多種方式以不同的集合控制項 (也可能稱為集合檢視) 來表示。 集合控制項會顯示並實現與集合型內容的互動，例如連絡人清單、日期清單、影像集合等等。

> **重要 API**：[ListView 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)、[GridView 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)、[FlipView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flipview)、[TreeView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)、[ItemsRepeater 類別](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)

本文涵蓋的控制項包括：

- 清單檢視，主要用來顯示大量文字內容集合
- 資料格檢視，主要用來顯示大量影像內容集合
- 翻轉視圖，主要用來顯示有大量影像內容、且一次只需要聚焦在一個項目的集合
- 樹狀檢視，主要用來顯示特定階層中有大量影像內容的集合
- ItemsRepeater，可用來建立自訂集合控制項的可自訂建置組塊

下面會提供每個控制項的設計指導方針、功能和範例。

每個控制項 (ItemsRepeater 除外) 都會提供內建的樣式和互動。 不過，若要進一步自訂集合檢視及其內部項目的視覺外觀，請使用 [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)。 如需資料範本和集合檢視外觀自訂方式的詳細資訊，請參閱[項目容器和範本](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/item-containers-templates)頁面。

每個控制項 (ItemsRepeater 除外) 也都有內建行為，可讓您選取單一或多個項目。 如需深入了解，請參閱[選取模式概觀](selection-modes.md)。

> **Windows 10 Fall Creators 更新 - 行為變更**根據預設，主動式手寫筆現在會在 UWP 應用程式中捲動/移動瀏覽清單 (如同觸控、觸控板和被動式手寫筆)，而不會執行選取。
> 如果您的應用程式需仰賴先前的行為，則可以覆寫手寫筆捲動並還原至先前的行為。 如需詳細資料，請參閱針對 [ScrollViewer 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) 的 API 參照主題。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請參閱 <a href="xamlcontrolsgallery:/item/ListView">ListView</a>、<a href="xamlcontrolsgallery:/item/GridView">GridView</a>、<a href="xamlcontrolsgallery:/item/FlipView">FlipView</a>、<a href="xamlcontrolsgallery:/item/TreeView">TreeView</a> 和 <a href="xamlcontrolsgalley:/item/ItemsRepeater">ItemsRepeater</a> 的實際運作。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="list-views"></a>清單檢視

清單檢視可表示有大量文字的項目，其通常具有單一資料行的垂直堆疊版面配置。 清單檢視可讓您將項目分類並指派群組標頭、拖放項目、規劃內容，以及重新排序項目。

### <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用清單檢視：

- 顯示主要由文字型項目組成的集合，且這些項目全都應該有相同的視覺和互動行為。
- 表示單一或分類的內容集合。
- 因應各種使用案例，包括下列常見情況：
    - 建立訊息或訊息記錄的清單。
    - 建立連絡人清單。
    - 在[主要/詳細資料模式](master-details.md)中建立主要窗格。 主要/詳細資料是經常用於電子郵件應用程式的模式，其中一個窗格 (主要) 具有可選取項目的清單，而另一個窗格 (詳細資料) 具有已選取項目的詳細資料檢視。
    

### <a name="examples"></a>範例

下面的簡單清單視圖會顯示連絡人清單，並依字母順序將資料項目分組。 群組標頭 (在此範例中為每個英文字母) 也可以自訂為保持「固定」，讓其在捲動時一律會出現在 ListView 頂端。

![具有分組資料的清單檢視](images/listview-grouped-example-resized-final.png)

這是會顯示訊息記錄的已反轉 ListView，其會讓最新的訊息出現在底部。 透過已反轉的 ListView，項目就會出現在畫面底部，並具有內建動畫。

![已反轉的清單檢視](images/listview-inverted-2.png)

### <a name="related-articles"></a>相關文章
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
<td align="left"><p>您以清單或方格檢視顯示的項目，將能在應用程式整體外觀上扮演重要的角色。 透過修改控制項範本和資料範本來自訂集合項目的外觀，讓應用程式擁有良好外觀。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">清單檢視的項目範本</a></p></td>
<td align="left"><p>使用這些適用於 ListView 的範例項目範本來取得常見應用程式類型的外觀。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">反轉清單</a></p></td>
<td align="left"><p>反轉清單會從底部開始加入新項目，例如聊天應用程式。 請遵循此文章的指導方針以在應用程式中使用反轉清單。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">拖動以重新整理</a></p></td>
<td align="left"><p>拖動以重新整理的機制可讓使用者以觸控的方式將資料清單向下拖動以抓取更多資料。 請使用此文章的指導方針以在清單檢視中實作拖動重新整理。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">巢狀 UI</a></p></td>
<td align="left"><p>巢狀 UI 是一種使用者介面 (UI)，能夠公開包含在容器中的可動作控制項，供使用者採取動作。 例如，您有一個包含按鈕的清單檢視項目，而使用者可以選取該清單項目，或按下巢嵌在其中的按鈕。 請遵循這些最佳做法來為使用者提供最佳的巢狀 UI 體驗。</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>方格檢視

資料格檢視適合用來排列和瀏覽以影像為基礎的內容集合。 資料格檢視版面配置垂直捲動和水平移動瀏覽。 項目會採用換行的版面配置，其會以由左至右、由上而下的讀取順序出現。

### <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用方格檢視來：

- 顯示內容集合，在此集合中，每個項目的焦點都是一個影像，而且每個項目都應該有相同的視覺和互動行為。
- 顯示內容庫。
- 將與[語意式縮放](semantic-zoom.md)關聯的兩個內容檢視格式化。
- 因應各種使用案例，包括下列常見情況：
    - 店面類型的使用者介面 (即瀏覽應用程式、歌曲、產品)
    - 互動式圖庫

### <a name="examples"></a>範例

這個範例顯示典型的資料格檢視版面配置 ，在此案例中是針對瀏覽應用程式。 資料格檢視項目的中繼資料通常受限於幾行文字與項目評等。

![資料格檢視配置範例](images/controls_gridview_example02.png)

資料格檢視是內容庫的理想解決方案，通常用來呈現如圖片和視訊等等的媒體。 在內容庫中，使用者預期能夠點選項目來叫用動作。

![內容庫範例](images/gridview-simple-example-final.png)

### <a name="related-articles"></a>相關文章
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
<td align="left"><p>您以清單或方格檢視顯示的項目，將能在應用程式整體外觀上扮演重要的角色。 透過修改控制項範本和資料範本來自訂集合項目的外觀，讓應用程式擁有良好外觀。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">方格檢視的項目範本</a></p></td>
<td align="left"><p>使用這些適用於 GridView 的範例項目範本來取得常見應用程式類型的外觀。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">巢狀 UI</a></p></td>
<td align="left"><p>巢狀 UI 是一種使用者介面 (UI)，能夠公開包含在容器中的可動作控制項，供使用者採取動作。 例如，您有一個包含按鈕的方格檢視項目，而使用者可以選取該方格項目，或按下巢嵌在其中的按鈕。 請遵循這些最佳做法來為使用者提供最佳的巢狀 UI 體驗。</p></td>
</tr>
</tbody>
</table>

## <a name="flip-views"></a>翻轉檢視

翻轉檢視適用於瀏覽影像型內容集合，特別是在一次只需要顯示一個影像的體驗時。 翻轉檢視可讓使用者移動過或快速「翻轉」過集合項目 (垂直或水準)，讓每個項目在使用者有所互動後一次只出現一個。

### <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用翻轉檢視來：

- 顯示小型到中型 (少於 25 個項目) 的集合，這些集合會由幾乎不含中繼資料的影像所組成。
- 一次顯示一個項目，並讓使用者依自己的步調來翻轉過每個項目。
- 因應各種使用案例，包括下列常見情況：
    - 圖庫
    - 產品庫或展示

### <a name="examples"></a>範例

下列兩個範例會分別顯示水平和垂直翻轉的 FlipView。

![水平翻轉檢視](images/controls_flipview_horizonal.jpg)

![垂直翻轉檢視](images/controls_flipview_vertical.jpg)

### <a name="related-articles"></a>相關文章
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
<td align="left"><p><a href="flipview.md">翻轉檢視</a></p></td>
<td align="left"><p>了解在應用程式中使用翻轉檢視的基本知識，以及如何在翻轉檢視內自訂項目外觀。</p></td>
</tr>
</tbody>
</table>

## <a name="tree-views"></a>樹狀檢視

樹狀檢視適用於顯示文字型集合，這些集合具有必須加以展示的重要階層。 樹狀檢視項目可折疊/展開、會以視覺階層顯示、可使用圖示來加以補充，並可在樹狀檢視之間拖放。 樹狀檢視允許 N 層巢狀結構。

### <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用樹狀檢視來：

- 顯示巢狀項目的集合，這些項目的內容和意義取決於階層或特定組織鏈結。
- 因應各種使用案例，包括下列常見情況：
    - 檔案瀏覽器
    - 公司組織圖

### <a name="examples"></a>範例

以下是代表檔案總管的樹狀檢視範例，會顯示以圖示補充的眾多不同巢狀項目。

![具有圖示的樹狀檢視](images/treeview-icons.png)

### <a name="related-articles"></a>相關文章
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
<td align="left"><p><a href="tree-view.md">樹狀檢視</a></p></td>
<td align="left"><p>了解在應用程式中使用樹狀檢視的基本知識，以及如何自訂樹狀檢視內項目的外觀和互動行為。</p></td>
</tr>
</tbody>
</table>

## <a name="itemsrepeater"></a>ItemsRepeater

ItemsRepeater 與本頁面上所顯示的其餘集合控制項不同，其不會提供任何現成的樣式設定或互動，也就是單純放在頁面上，而不會定義任何屬性。 ItemsRepeater 就是建置組塊，可供身為開發人員的您用來建立自己的自訂集合控制項，特別是無法使用本文其他控制項來實現的控制項。 ItemsRepeater 是資料驅動的高效能面板，可根據您的確切需求量身打造。

### <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

如有下列情況，請使用 ItemsRepeater：

- 您已構思出具體的使用者介面和使用者體驗，但無法使用現有集合控制項加以建立。
- 您有項目的現有資料來源 (例如，從網際網路提取的資料、資料庫，或程式碼後置中的既有集合)。

### <a name="examples"></a>範例

下列三個範例是繫結至相同資料來源 (數字集合) 的所有 ItemsRepeater 控制項。 數字集合以三種方式表示，下列的每個 ItemsRepeater 會使用不同的自訂[版面配置](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.layout)和不同的自訂 [ItemTemplate](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate?view=winui-2.2)。

![具有橫條的 ItemsRepeater](images/itemsrepeater-1.png)
![具有直條的 ItemsRepeater](images/itemsrepeater-2.png)
![具有圓形表示的 ItemsRepeater](images/itemsrepeater-3.png)

### <a name="related-articles"></a>相關文章
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
<td align="left"><p><a href="items-repeater.md">ItemsRepeater</a></p></td>
<td align="left"><p>了解在應用程式中使用 ItemsRepeater 的基本知識，以及如何為集合檢視實作所有必要的互動和視覺元件。</p></td>
</tr>
</tbody>
</table>


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

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

**設計和 UX 指導方針**
- [主要/詳細資料](master-details.md)
- [瀏覽窗格](navigationview.md)
- [語意式縮放](semantic-zoom.md)
- [拖放功能](https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [縮圖影像](../../files/thumbnails.md)

**API 參考**
- [ListView 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) \(英文\)
- [ComboBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) \(英文\)
- [ListBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) \(英文\)
