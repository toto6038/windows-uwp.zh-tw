---
author: mijacobs
Description: 任何一種 app 的主要用途都是讓使用者存取內容。 在相片編輯 app 中，內容是相片；在旅遊 app 中，內容是地圖與旅遊地點的相關資訊；依此類推。
title: 通用 Windows 平台 (UWP) 應用程式的內容設計基本知識
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
---

#  UWP app 的內容設計基本知識

任何一種 app 的主要用途都是讓使用者存取內容：在相片編輯 app 中，內容是相片；在旅遊 app 中，內容是地圖與旅遊地點的相關資訊；以此類推。 瀏覽元素提供內容的存取；命令元素可以讓使用者與內容互動；內容元素顯示實際的內容。

本文提供針對三種內容案例的內容設計建議。

## <span id="Design_for_the_right_content_scenario"></span><span id="design_for_the_right_content_scenario"></span><span id="DESIGN_FOR_THE_RIGHT_CONTENT_SCENARIO"></span>正確的內容案例設計


有三種主要的內容案例：

-   **取用**：內容的取用，主要是單向的體驗。 取用包含從事閱讀、聆聽音樂、觀賞影片，以及檢視相片與影像等活動。
-   **建立**：焦點為建立新內容，主要是單向的體驗。 建立可以分成從無到有的創作，例如拍攝相片或影片、在繪圖應用程式中建立新的影像或開啟新的文件。
-   **互動式**：包含取用、建立及修改內容的雙向內容體驗。

## <span id="Consumption-focused_apps"></span><span id="consumption-focused_apps"></span><span id="CONSUMPTION-FOCUSED_APPS"></span>以取用為主的 app


內容元素在取用為主的app 中擁有最高的優先權，接著需要[瀏覽元素](navigation-basics.md)來協助使用者尋找想要的內容。 取用為主的應用程式範例包括影片播放程式、閱讀應用程式、音樂應用程式，以及相片檢視器等等。

![新聞閱讀應用程式](images/news-reader/v2/newsreader-v2-tablet-phone.png)

針對取用為主應用程式的一般建議：

-   請考慮建立專用的[瀏覽](navigation-basics.md)頁面以及內容檢視頁面，如此當使用者尋找想要的內容時，可以在專用的頁面檢視，免去其餘的干擾。
-   請考慮建立全螢幕檢視選項，它可以將內容展開到整個螢幕，並隱藏其他的 UI 元素。

## <span id="Creation-focused_apps"></span><span id="creation-focused_apps"></span><span id="CREATION-FOCUSED_APPS"></span>建立為主的 app


在建立為主的 app 中，內容和[命令](commanding-basics.md)元素是最重要的 UI 元素：命令元素讓使用者可以建立新內容。 範例包括繪圖應用程式、相片編輯應用程式、影片編輯應用程式，以及文書處理應用程式。

例如，以下是設計使用命令列來提供工具存取和相片操作選項的相片應用程式。 因為所有的命令都放在命令列中，應用程式能為相片內容和編輯提供大部分的螢幕空間。

![使用主動式畫布的相片編輯應用程式範例](images/photo-editor/uap-photo-tabletphone-sbs.png)

針對建立為主應用程式的一般建議：

-   最小化[瀏覽](navigation-basics.md)元素的使用。
-   [命令](commanding-basics.md)元素在建立為主的應用程式中特別重要。 因為使用者會執行大量的命令，我們建議提供命令記錄/復原功能。

## <span id="Apps_with_interactive_content"></span><span id="apps_with_interactive_content"></span><span id="APPS_WITH_INTERACTIVE_CONTENT"></span>具有互動式內容的應用程式


在具有互動式內容的應用程式中，使用者可以建立、檢視和編輯內容；許多應用程式都屬於這個類別。 這類型的應用程式範例包括企業營運應用程式、庫存管理應用程式，可讓使用者建立和編輯食譜的烹飪應用程式等等。

![共同作業工具設計，具有互動式內容的應用程式](images/collaboration-tool/uap-collaboration-tabphone-700.png)

這類的應用程式，需要在全部三種 UI 元素的使用中取得平衡：

-   [瀏覽](navigation-basics.md)元素協助使用者尋找及檢視內容。 如果檢視與尋找內容是最重要的案例，請排列瀏覽元素優先順序，先篩選與排序，然後是搜尋。
-   [命令](commanding-basics.md)元素可讓使用者建立、編輯及操控內容。

針對包含互動式內容之應用程式的一般建議：

-   瀏覽、內容和命令元素，當它們三者都很重要時是難以平衡的。 可能的話，請考慮針對瀏覽、建立和編輯內容建立個別的視窗，或提供模式切換。

## <span id="Commonly_used_content_elements"></span><span id="commonly_used_content_elements"></span><span id="COMMONLY_USED_CONTENT_ELEMENTS"></span>常用的內容元素


以下是一些常用來顯示內容的 UI 元素。 (如需 UI 元素的完整清單，請參閱 [Controls and UI elements](https://msdn.microsoft.com/library/windows/apps/dn611856) (控制項與 UI 元素)。)

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
<td align="left">音訊與視訊</td>
<td align="left">[媒體播放和傳輸控制項](../controls-and-patterns/media-playback.md)</td>
<td align="left">播放音訊與視訊。</td>
</tr>
<tr class="even">
<td align="left">影像檢視器</td>
<td align="left">[翻轉檢視](../controls-and-patterns/flipview.md)、[影像](../controls-and-patterns/images-imagebrushes.md)</td>
<td align="left">顯示影像。 翻轉檢視顯示集合中的影像 (一次一個影像)，例如相簿中的相片或是產品詳細資料頁面中的項目。</td>
</tr>
<tr class="odd">
<td align="left">清單</td>
<td align="left">[下拉式清單、清單方塊、清單檢視與方格檢視](../controls-and-patterns/lists.md)</td>
<td align="left">在互動式清單或方格中呈現項目。 使用這些元素，可讓使用者從最新發行的清單中選取電影，或是管理詳細目錄。</td>
</tr>
<tr class="even">
<td align="left">文字和輸入文字</td>
<td align="left"><p>[文字區塊](../controls-and-patterns/text-block.md)、[文字方塊](../controls-and-patterns/text-box.md)、[可編輯對話方塊](../controls-and-patterns/rich-edit-box.md)</p>
</td>
<td align="left">顯示文字。 某些元素可讓使用者編輯文字。 如需詳細資訊，請參閱[文字控制項](../controls-and-patterns/text-controls.md)</td>
</tr>
</tbody>
</table>



 

 






<!--HONumber=May16_HO2-->


