---
author: Karl-Bridge-Microsoft
Description: "處理用於觸控和手寫筆輸入的相同基本指標事件，即可在 app 中回應滑鼠輸入。"
title: "滑鼠互動"
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: b381cdba2a20480bb8b4d7802d2ff490744e5f03

---

# 滑鼠互動


針對觸控輸入最佳化您的通用 Windows 平台 (UWP) app，獲得預設的基本滑鼠支援。

 

![滑鼠](images/input-patterns/input-mouse.jpg)



滑鼠輸入最適合要求精確指向與按一下的使用者互動。 Windows 的 UI 理所當然支援這種本有的精確度，Windows 8 已針對觸控的不精確本質最佳化。

滑鼠和觸控輸入的區別在於觸控能夠透過直接在這些物件上實際運用手勢 (如撥動、滑動、拖曳、旋轉等等)，在畫面上更仔細地模擬對 UI 元素的直接操作。 使用滑鼠操作通常需要一些其他 UI 能供性，例如使用控制代碼以調整物件大小或旋轉物件。

這個主題說明滑鼠互動的設計考量。

## <span id="The_UWP_app_mouse_language"></span><span id="the_uwp_app_mouse_language"></span><span id="THE_UWP_APP_MOUSE_LANGUAGE"></span>UWP app 滑鼠語言


一組可用於整個系統的簡單滑鼠互動。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">詞彙</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Hover_to_learn"></span><span id="hover_to_learn"></span><span id="HOVER_TO_LEARN"></span>暫留以了解</p></td>
<td align="left"><p>暫留於某個元素上方可在不進行任何動作下，顯示更詳細的資訊或教學視覺物件 (例如工具提示)。</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Left-click_for_primary_action"></span><span id="left-click_for_primary_action"></span><span id="LEFT-CLICK_FOR_PRIMARY_ACTION"></span>按滑鼠左鍵以執行主要動作</p></td>
<td align="left"><p>在元素上按滑鼠左鍵即可叫用它的主要動作 (例如啟動應用程式或執行命令)。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="Scroll_to_change_view"></span><span id="scroll_to_change_view"></span><span id="SCROLL_TO_CHANGE_VIEW"></span>捲動以變更檢視</p></td>
<td align="left"><p>顯示捲軸，以在內容區域內上、下、左、右移動。 使用者可以按捲軸或者滾動滑鼠滾輪來進行捲動。 捲軸可以指出目前檢視在內容區域內的位置 (利用觸控進行移動瀏覽會顯示相似的 UI)。</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Right-click_to_select_and_command"></span><span id="right-click_to_select_and_command"></span><span id="RIGHT-CLICK_TO_SELECT_AND_COMMAND"></span>按滑鼠右鍵選取與命令</p></td>
<td align="left"><p>按滑鼠右鍵可顯示其中包含全域命令的瀏覽列 (如果有的話) 和應用程式列。 在某個元素上按一下滑鼠右鍵即可選取該元素，並且顯示其中包含所選元素之操作命令的應用程式列。</p>
<div class="alert">
<strong>注意：</strong>如果「選取」或「應用程式列命令」不是適當的 UI 行為，則以按滑鼠右鍵來顯示操作功能表。 我們強烈建議您針對所有命令行為使用應用程式列。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="UI_commands_to_zoom"></span><span id="ui_commands_to_zoom"></span><span id="UI_COMMANDS_TO_ZOOM"></span>用於縮放的 UI 命令</p></td>
<td align="left"><p>在應用程式列中顯示 UI 命令 (例如 + 和 -)，或者按 Ctrl 並滾動滑鼠滾輪，以模擬用於縮放的捏合和伸展手勢。</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="UI_commands_to_rotate"></span><span id="ui_commands_to_rotate"></span><span id="UI_COMMANDS_TO_ROTATE"></span>用於旋轉的 UI 命令</p></td>
<td align="left"><p>在應用程式列中顯示 UI 命令，或者按 Ctrl+Shift 並滾動滑鼠滾輪，以模擬用於旋轉的轉動手勢。 旋轉裝置本身即可旋轉整個螢幕。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="Left-click_and_drag_to_rearrange"></span><span id="left-click_and_drag_to_rearrange"></span><span id="LEFT-CLICK_AND_DRAG_TO_REARRANGE"></span>按滑鼠左鍵並拖曳以重新排列</p></td>
<td align="left"><p>按滑鼠左鍵並拖曳元素來移動元素。</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Left-click_and_drag_to_select_text"></span><span id="left-click_and_drag_to_select_text"></span><span id="LEFT-CLICK_AND_DRAG_TO_SELECT_TEXT"></span>按滑鼠左鍵並拖曳以選取文字</p></td>
<td align="left"><p>在可選取的文字內按滑鼠左鍵並拖曳即可選取文字。 按兩下即可選取某個字。</p></td>
</tr>
</tbody>
</table>

## 滑鼠事件

處理用於觸控和手寫筆輸入的相同基本指標事件，即可在 app 中回應滑鼠輸入。

使用 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 事件實作基本的輸入功能，不必為每一種指標輸入裝置撰寫程式碼。 不過，您仍然可以使用此物件的指標、手勢與操作事件，利用每一種裝置的特殊功能 (例如滑鼠滾輪事件)。

**範例：**在 [App 範例](http://go.microsoft.com/fwlink/p/?LinkID=264996)中，查看此功能的執行方式。


- [輸入：裝置功能範例](http://go.microsoft.com/fwlink/p/?linkid=231530)

- [輸入範例](http://go.microsoft.com/fwlink/p/?linkid=226855)

- [輸入：使用 GestureRecognizer 處理手勢與操作](http://go.microsoft.com/fwlink/p/?LinkID=231605)

## <span id="Guidelines_for_visual_feedback"></span><span id="guidelines_for_visual_feedback"></span><span id="GUIDELINES_FOR_VISUAL_FEEDBACK"></span>視覺化回饋的指導方針


-   偵測到滑鼠時 (透過移動或暫留事件)，顯示滑鼠特定 UI，指示元素公開的功能。 如果滑鼠有一段時間沒有移動，或者使用者起始觸控互動，讓滑鼠 UI 逐漸淡出。 這可以讓 UI 保持整齊、不凌亂。
-   請勿為暫留回饋使用游標，元素提供的回饋已經足夠 (請參閱下方的游標說明)。
-   如果元素不支援互動 (例如靜態文字)，請勿顯示視覺化回饋。
-   請勿搭配滑鼠互動使用焦點矩形。 請保留這些給鍵盤互動。
-   如果所有元素均代表相同的輸入目標，請同時顯示視覺化回饋。
-   提供模擬觸控式操作 (例如移動瀏覽、旋轉、縮放等等) 的按鈕 (例如 + 和 -)。

如需視覺化回饋的詳細一般指導方針，請參閱[視覺化回饋的指導方針](guidelines-for-visualfeedback.md)。


## <span id="Cursors"></span><span id="cursors"></span><span id="CURSORS"></span>游標


我們提供了一組可用於滑鼠指標的標準游標。 它們可用來指示元素的主要動作。

每一個標準游標都有與其關聯之相對應的預設影像。 使用者或 app 可以隨時取代與任何標準游標相關聯的預設影像。 透過 [**PointerCursor**](https://msdn.microsoft.com/library/windows/apps/br208273) 函式指定游標影像。

如果您需要自訂滑鼠游標：

-   一律使用箭頭游標 (![箭頭游標](images/cursor-arrow.png)) 於可點選的元素。 請勿使用指向手型游標 (![指向手型游標](images/cursor-pointinghand.png)) 於連結或其他互動式元素。 請改為使用暫留效果 (描述如前)。
-   使用文字游標 (![文字游標](images/cursor-text.png)) 於可選取的文字。
-   使用移動游標 (![移動游標](images/cursor-move.png)) 於主要動作為移動時 (例如拖曳或裁剪時)。 對於主要動作為瀏覽的元素 (例如 \[開始\] 畫面磚)，請勿使用移動游標。
-   請使用水平、垂直及對角線調整游標 (![調整垂直大小游標](images/cursor-vertical.png)、 ![調整水平大小游標](images/cursor-horizontal.png)、 ![對角線調整游標 (左下右上)](images/cursor-diagonal2.png)、 ![對角線調整游標 (左上右下)](images/cursor-diagonal1.png)) 於物件可調整時。
-   使用握拳游標 (![握拳游標 (打開)](images/cursor-pan1.png)、 ![握拳游標 (握緊)](images/cursor-pan2.png)) 於固定畫布 (例如地圖) 內移動瀏覽內容時。

## <span id="related_topics"></span>相關文章

* [處理指標輸入](handle-pointer-input.md)
* [識別輸入裝置](identify-input-devices.md)

**範例**
* [基本輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延遲輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [使用者互動模式範例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**封存範例**
* [輸入：裝置功能範例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [輸入：XAML 使用者輸入事件範例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [XAML 捲動、移動瀏覽和縮放範例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [輸入：使用 GestureRecognizer 處理手勢與操作](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 
 

 







<!--HONumber=Jun16_HO3-->


