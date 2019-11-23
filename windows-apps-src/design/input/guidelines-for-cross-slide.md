---
Description: 使用交叉滑動以支援透過撥動手勢進行選取，以及透過滑動手勢進行拖曳 (移動) 互動。
title: 交叉滑動的指導方針
ms.assetid: 897555e2-c567-4bbe-b600-553daeb223d5
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 833949effd311c707de8dd1823ec6eee06e91e87
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257980"
---
# <a name="guidelines-for-cross-slide"></a>交叉滑動的指導方針




**重要 API**

-   [**CrossSliding**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.crosssliding)
-   [**CrossSlideThresholds**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.crossslidethresholds)
-   [**Windows. UI. 輸入**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)

使用交叉滑動以支援透過撥動手勢進行選取，以及透過滑動手勢進行拖曳 (移動) 互動。

## <a name="span-iddos_and_don_tsspanspan-iddos_and_don_tsspanspan-iddos_and_don_tsspandos-and-donts"></a><span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Dos 和不應事項


-   針對朝單一方向捲動的清單或集合使用交叉滑動。
-   當點選互動用於其他用途時，項目選取請使用交叉滑動。
-   不要使用交叉滑動將項目新增到佇列。

## <a name="span-idadditional_usage_guidancespanspan-idadditional_usage_guidancespanspan-idadditional_usage_guidancespanadditional-usage-guidance"></a><span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>其他使用指引


選取和拖曳只適用於單一方向移動瀏覽 (垂直或水平) 的內容區域。 為了讓任一種互動可以作用，必須鎖定單一移動瀏覽方向，而且必須以與移動瀏覽方向垂直的方向操作手勢。

以下示範使用交叉滑動來選取和拖曳物件的做法。 左邊的影像顯示在撥動手勢提起並放開物件之前，如果未超出距離閾值，項目就會被選取。 右邊的影像顯示滑動手勢超出距離閾值，而導致物件被施曳。

![顯示選取和拖放程序的圖。](images/crossslide-mechanism.png)

交叉滑動互動所使用的閾值距離如下圖所示。

![顯示選取和拖放程序的螢幕擷取畫面。](images/crossslide-threshold.png)

若要保留移動瀏覽功能，必須先超出一個 2.7mm (約等於目標解析度 10 像素) 的小閾值，才能啟動選取或拖曳互動。 這個小閾值可以協助系統區別交叉滑動與移動瀏覽的不同，同時也可以協助系統確保能夠辨識出點選手勢與交叉滑動及移動瀏覽的不同。

此圖顯示使用者如何觸控 UI 中的一個元素，卻在手指接觸時稍微往下移了一些。 如果沒有設定閾值，因為一開始是垂直移動，所以這項互動會被解譯為交叉滑動。 如果有設定閾值，這項移動就會被正確解譯為水平移動瀏覽。

![顯示選取和拖放判別閾值的螢幕擷取畫面。](images/crossslide-threshold2.png)

以下是在 app 中包含交叉滑動功能時需要考量的一些指導方針。

針對朝單一方向捲動的清單或集合使用交叉滑動。 如需詳細資訊，請參閱[新增 ListView 控制項](https://docs.microsoft.com/previous-versions/windows/apps/hh465382(v=win.10))。

**請注意**  在內容區域可透過兩個方向移動流覽的情況下（例如網頁瀏覽器或電子郵件讀取器），您應該使用按住計時互動來叫用影像和超連結等物件的快顯功能表。

 

|                                                                                         |                                                                                         |
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| ![水平移動瀏覽，二維清單](images/groupedlistview1.png)                | ![垂直移動瀏覽，一維清單](images/listviewlistlayout.png)                |
| 一個水平移動瀏覽的二維清單。 垂直拖曳以選取或移動項目。 | 一個垂直移動瀏覽的一維清單。 水平拖曳以選取或移動項目。 |

 

### <span id="selection"></span><span id="SELECTION"></span>

**選取**

選取是對一或多個物件做標示，而不啟動或啟用。 這個動作與使用滑鼠按一下或按住 Shift 鍵並用滑鼠按一下一或多個物件類似。

交叉滑動選取的執行方式是觸碰元素，並在短暫的拖曳互動後再放開。 這個選取方法同時省去其他觸控介面所需的專用選取模式和長按計時互動，而且與用於啟動的點選互動並不衝突。

除了距離閾值之外，交叉滑動選取也受限於 90° 閾值區域，如下圖所示。 如果拖出這個區域範圍，物件就不會被選取。

![顯示選取閾值區域的圖。](images/crossslide-selection.png)

交叉滑動互動可以補充長按計時互動 (也稱為「自顯」互動)。 這項補充的互動會啟動一個動畫，指示可在物件上執行的動作。 如需有關判別 UI 的詳細資訊，請參閱[視覺化回饋的指導方針](guidelines-for-visualfeedback.md)。

下列螢幕擷取畫面示範自顯動畫的運作方式。

1.  長按以初始自顯互動的動畫。 選取的項目狀態會影響動畫所顯示的內容：如果取消選取就會顯示勾號，如果選取則不會顯示勾號。

    ![顯示處於未選取狀態的螢幕擷取畫面。](images/crossslide-selfreveal1.png)

2.  使用撥動手勢 (向上或向下) 選取項目。

    ![顯示選取動畫的螢幕擷取畫面。](images/crossslide-selfreveal2.png)

3.  目前已選取項目。 使用滑動手勢來移動項目，以覆寫選取行為。

    ![顯示拖放動畫的螢幕擷取畫面。](images/crossslide-selfreveal3.png)

在將選取做為唯一主要動作的應用程式中使用單一點選。 顯示交叉滑動自顯動畫是為了讓使用者判別這項功能與用於啟動和瀏覽的標準點選互動不同。

**選取籃**

選取項目籃可以清楚且動態地顯示已經從應用程式的主要清單或集合選取的項目。 這個功能適合用來追蹤選取的項目，而且在以下情況時，應用程式應該加以利用：

-   可以從多個位置選取項目。
-   可以選取多個項目。
-   動作或命令依賴選取的清單。

選取項目籃的內容並不會因動作和命令變更而消失。 例如，如果您從藝廊中選取了一系列相片、對每張相片套用色彩校正，然後以某種方式分享相片，那些項目仍然會處於選取狀態。

如果未在應用程式中使用選取項目籃，則在執行動作或命令後，會將目前的選項清除。 例如，如果您從播放清單中選取了一首歌並予以分級，就會將該選項清除。

當未使用選取項目籃並且已經啟動清單或集合中的另一個項目時，也會清除目前的選項。 例如，如果您選取了某個收件匣訊息，預覽窗格就會更新。 接著，如果您選取了第二個收件匣訊息，就會取消選取上一個訊息，並且預覽窗格也會更新。

**等待**

佇列不等於選取項目籃清單，不應該當作是選取項目籃清單。 主要差別包括：

-   選取項目籃中的項目清單只是視覺化呈現；佇列中的項目是利用使用者指定的特定動作而組合在一起。
-   項目只能在選取項目籃中呈現一次，但是可以在佇列中呈現多次。
-   選取項目籃中的項目順序代表選取順序。 佇列中的項目順序與功能直接相關。

因為這些理由，所以不應該使用交叉滑動選取互動將項目新增到佇列。 而是應該透過拖曳動作將項目新增到佇列。

### <span id="draganddrop"></span><span id="DRAGANDDROP"></span>

**拖放式**

使用拖曳，將一或多個物件從一個位置移到另一個位置。

如果需要移動的物件超過一個，請讓使用者選取多個項目，然後同時拖曳所有項目。

## <a name="span-idrelated_topicsspanrelated-articles"></a><span id="related_topics"></span>相關文章


**範例**
* [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
**封存範例**
* [輸入： XAML 使用者輸入事件範例](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [輸入：裝置功能範例](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [輸入：觸控點擊測試範例](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [XAML 捲軸、移動流覽和縮放範例](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [輸入：簡化的筆跡範例](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [輸入： Windows 8 手勢範例](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Input：操作和手勢（C++）範例](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [DirectX touch 輸入範例](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




