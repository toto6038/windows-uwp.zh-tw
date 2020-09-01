---
Description: 處理用於觸控和手寫筆輸入的相同基本指標事件，即可在 app 中回應滑鼠輸入。
title: 滑鼠互動
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 242f7c30260956bc84478153f39b0da4d8461e12
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173402"
---
# <a name="mouse-interactions"></a>滑鼠互動

針對觸控輸入優化 Windows 應用程式設計，並依預設取得基本的滑鼠支援。 

![滑鼠](images/input-patterns/input-mouse.jpg)

滑鼠輸入最適合在指向及點選方面要求精確的使用者互動。 Windows 的 UI 已針對觸控的不精確本質進行最佳化，可自然地支援這種固有的精確度。

滑鼠和觸控輸入的區別在於觸控能夠透過直接在這些物件上實際運用手勢 (如撥動、滑動、拖曳、旋轉等等)，在畫面上更仔細地模擬對 UI 元素的直接操作。 使用滑鼠操作通常需要一些其他 UI 能供性，例如使用控制代碼以調整物件大小或旋轉物件。

這個主題說明滑鼠互動的設計考量。

## <a name="the-uwp-app-mouse-language"></a>UWP app 滑鼠語言

一組可用於整個系統的簡單滑鼠互動。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">詞彙</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>暫留以了解</p></td>
<td align="left"><p>暫留於某個元素上方可在不進行任何動作下，顯示更詳細的資訊或教學視覺物件 (例如工具提示)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>按滑鼠左鍵以執行主要動作</p></td>
<td align="left"><p>在元素上按滑鼠左鍵即可叫用它的主要動作 (例如啟動應用程式或執行命令)。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>捲動以變更檢視</p></td>
<td align="left"><p>顯示捲軸，以在內容區域內上、下、左、右移動。 使用者可以按捲軸或者滾動滑鼠滾輪來進行捲動。 捲軸可以指出目前檢視在內容區域內的位置 (利用觸控進行移動瀏覽會顯示相似的 UI)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>按滑鼠右鍵選取與命令</p></td>
<td align="left"><p>按滑鼠右鍵可顯示其中包含全域命令的瀏覽列 (如果有的話) 和應用程式列。 在某個元素上按一下滑鼠右鍵即可選取該元素，並且顯示其中包含所選元素之操作命令的應用程式列。</p>
<div class="alert">
<strong>注意</strong>   如果選取範圍或應用程式行命令不是適當的 UI 行為，請以滑鼠右鍵按一下以顯示操作功能表。 我們強烈建議您針對所有命令行為使用應用程式列。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>用於縮放的 UI 命令</p></td>
<td align="left"><p>在應用程式列中顯示 UI 命令 (例如 + 和 -)，或者按 Ctrl 並滾動滑鼠滾輪，以模擬用於縮放的捏合和伸展手勢。</p></td>
</tr>
<tr class="even">
<td align="left"><p>用於旋轉的 UI 命令</p></td>
<td align="left"><p>在應用程式列中顯示 UI 命令，或者按 Ctrl+Shift 並滾動滑鼠滾輪，以模擬用於旋轉的轉動手勢。 旋轉裝置本身即可旋轉整個螢幕。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>按滑鼠左鍵並拖曳以重新排列</p></td>
<td align="left"><p>按滑鼠左鍵並拖曳元素來移動元素。</p></td>
</tr>
<tr class="even">
<td align="left"><p>按滑鼠左鍵並拖曳以選取文字</p></td>
<td align="left"><p>在可選取的文字內按滑鼠左鍵並拖曳即可選取文字。 按兩下即可選取某個字。</p></td>
</tr>
</tbody>
</table>

## <a name="mouse-input-events"></a>滑鼠輸入事件

大部分的滑鼠輸入都可以透過所有 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 物件所支援的一般路由輸入事件來處理。 其中包含：

- [**BringIntoViewRequested**](/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**CoNtextCanceled**](/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**CoNtextRequested**](/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**下降**](/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Holding**](/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefound)
- [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**System.windows.forms.control.previewkeydown>**](/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**PreviewKeyUp**](/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**RightTapped**](/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped)

不過，您可以利用每個裝置的特定功能 (例如滑鼠滾輪事件，) 使用 [Windows](/uwp/api/windows.ui.input)中的指標、手勢和操作事件。

**範例：** 請參閱我們的 [BasicInput 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)，適用于。

## <a name="guidelines-for-visual-feedback"></a>視覺化回饋的指導方針

- 偵測到滑鼠時 (透過移動或暫留事件)，顯示滑鼠特定 UI，指示元素公開的功能。 如果滑鼠有一段時間沒有移動，或者使用者起始觸控互動，讓滑鼠 UI 逐漸淡出。 這可以讓 UI 保持整齊、不凌亂。
- 請勿為暫留回饋使用游標，元素提供的回饋已經足夠 (請參閱下方的游標說明)。
- 如果元素不支援互動 (例如靜態文字)，請勿顯示視覺化回饋。
- 請勿搭配滑鼠互動使用焦點矩形。 請保留這些給鍵盤互動。
- 如果所有元素均代表相同的輸入目標，請同時顯示視覺化回饋。
- 提供模擬觸控式操作 (例如移動瀏覽、旋轉、縮放等等) 的按鈕 (例如 + 和 -)。

如需視覺化回饋的詳細一般指導方針，請參閱[視覺化回饋的指導方針](guidelines-for-visualfeedback.md)。

## <a name="cursors"></a>資料指標

我們提供了一組可用於滑鼠指標的標準游標。 它們可用來指示元素的主要動作。

每一個標準游標都有與其關聯之相對應的預設影像。 使用者或應用程式可以隨時取代與任何標準游標相關聯的預設影像。 透過 [**PointerCursor**](/uwp/api/windows.ui.core.corewindow.pointercursor) 函式指定游標影像。

如果您需要自訂滑鼠游標：

- 一律使用箭頭游標 (![箭頭游標](images/cursor-arrow.png)) 於可點選的元素。 請勿使用指向手型游標 (![指向手型游標](images/cursor-pointinghand.png)) 於連結或其他互動式元素。 請改為使用暫留效果 (描述如前)。
- 使用文字游標 (![文字游標](images/cursor-text.png)) 於可選取的文字。
- 使用移動游標 (![移動游標](images/cursor-move.png)) 於主要動作為移動時 (例如拖曳或裁剪時)。 對於主要動作為瀏覽的元素 (例如 [開始] 畫面磚)，請勿使用移動游標。
- 請使用水平、垂直及對角線調整游標 (![調整垂直大小游標](images/cursor-vertical.png), ![調整水平大小游標](images/cursor-horizontal.png), ![對角線調整游標 (左下右上)](images/cursor-diagonal2.png), ![對角線調整游標 (左上右下)](images/cursor-diagonal1.png)) 於物件可調整時。
- 使用握拳游標 (![握拳游標 (打開)](images/cursor-pan1.png), ![握拳游標 (握緊)](images/cursor-pan2.png)) 於固定畫布 (例如地圖) 內移動瀏覽內容時。

## <a name="related-articles"></a>相關文章

- [處理指標輸入](handle-pointer-input.md)
- [識別輸入裝置](identify-input-devices.md)
- [事件與路由事件概觀](../../xaml-platform/events-and-routed-events-overview.md)

### <a name="samples"></a>範例

- [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)