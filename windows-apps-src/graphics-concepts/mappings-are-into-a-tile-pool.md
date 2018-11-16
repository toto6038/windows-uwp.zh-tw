---
title: 對應在磚集區中
description: 將資源建立為串流資源時，組成資源的磚來自指向磚集區中的位置。 磚集區是記憶體的集區 (場景後方由一或多個配置備份 - 應用程式中看不見)。
ms.assetid: 58B8DBD5-62F5-4B94-8DD1-C7D57A812185
keywords:
- 對應在磚集區中
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 24c8787efd108acb2353f6705dbb65a34d358ef2
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6844484"
---
# <a name="mappings-are-into-a-tile-pool"></a>對應在磚集區中


將資源建立為串流資源時，組成資源的磚來自指向磚集區中的位置。 磚集區是記憶體的集區 (場景後方由一或多個配置備份 - 應用程式中看不見)。 作業系統和顯示器驅動程式管理這個記憶體集區，讓應用程式容易了解磁碟使用量。 指向磚集區中的位置，串流資源對應 64 KB 地區。 這項設定的一個附帶結果是，它可以讓多資源共用及重複使用相同的動態磚，如有需要，也讓相同的動態磚在資源內的不同位置重複使用。

從磚集區彈性填入磚的成本是，該資源必須定義和維護磚集區中的磚對應代表資源所需的動態磚。 您可以變更磚對應。 並非所有資源中的磚每次都需要對應一次。資源可以有 **NULL** 對應。 **NULL** 對應定義為從存取之資源的觀點無法使用磚。

您可以建立多個磚集區，任何串流資源數可在同時間對應至任何特定的磚集區。 磚集區也可以變大或縮小。 如需詳細資訊，請參閱[調整磚集區大小](tile-pool-resizing.md)。 簡化顯示器驅動程式及執行階段實作的一個既存限制是，特定串流資源每次頂多只能對應到一個磚集區 (而不需要同時對應多個磚集區)。

串流資源本身 (也就是獨立磚集區記憶體) 相關聯的儲存量，與任何時候實際對應至集區的磚數目大致成正比。 因此，對於硬體便需使用對應的磚數量大約調整分頁表儲存空間的磁碟使用量 (例如，視情況使用多層次頁面表格配置)。

磚集區可以視為完全軟體抽象，能使 Direct3D 應用程式有效地能有效地規劃圖形處理器 (GPU) 的網頁表格，而不需要知道低層實作細節 (或直接處理指標地址)。 在硬體中，磚集區不套用任何其他間接取值層級。 使用像是與磚集區概念無關的頁面目錄的建構的單層分頁表最佳化。

讓我們瀏覽分頁表本身在最糟情況可能要求的儲存體 (雖然實際上實作只需要和所對應者大約一致的儲存體)。

假設每個分頁表項目是 64 位元。

最糟的分頁表點擊單一表面，指定在 Direct3D11、 有資源限制，假設串流資源是每一元素 128 位元的格式建立 （例如，RGBA 浮點數），因此 64 KB 磚只包含 4096 素。 支援的最大值 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 大小 16384\ * 16384\ * 2048 (但只有單一 Mipmap) 如果使用 64 位元資料表項目完整填入 (不包括 Mipmap)，則分頁表需要約 1 GB 的儲存空間。 新增 Mipmap 會增加完全對應 (最糟情況) 分頁表儲存空間約 1.3GB 的三分之一。

這種情形存取約 10.6 TB 可定址記憶體。 但可能會限制可定址記憶體數量，這會使數量減少至約 TB 範圍。

另一可考慮情況是單一 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 串流資源 16384\*16384，具備每一元素 32 位元格式，包括 Mipmap。 完整填入分頁表所需的空間約 170 KB，具有 64 位元資料表項目。

最後，請考慮使用 BC 格式的範例，例如 BC7 含每個磚 128 位元 4x4 像素。 這是每個像素一個位元組。 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 的 16384\ * 16384\ * 2048 包括 Mipmap，需要約 85 MB 將此記憶體完全填入分頁表中。 也可以考慮讓一項串流資源跨越 550 gigapixels (在本案例 512 GB 以上)。

實際上，如果實體可用記憶體數量不允許每次都對應和參考，便不定義這些完整對應。 但若有磚集區，應用程式便可以選擇重複使用動態磚 (例如，重複使用「黑」色磚針對影像中大型塊黑色區域) - 有效使用磚集區 (也就是分頁表對應) 當做記憶體壓縮工具。

分頁表所有項目的初始內容為 **NULL**。 應用程式也無法傳遞表面的記憶體內容的初始資料，因為它一開始便沒有輔助記憶體。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本節內容


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
<td align="left"><p><a href="tile-pool-creation.md">建立磚集區</a></p></td>
<td align="left"><p>應用程式可以在每一個 Direct3D 裝置建立一個或多個磚集區。 每個磚集區的大小總和限制為 Direct3D11 的資源大小上限，也就是約 1/4 的 GPU RAM。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-pool-resizing.md">調整磚集區大小</a></p></td>
<td align="left"><p>如果應用程式需要更多串流資源對應至工作集，重新調整磚集區的大小以增加磚集區，如果需要較少空間則縮小。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="hazard-tracking-versus-tile-pool-resources.md">危險追蹤與磚集區資源</a></p></td>
<td align="left"><p>對於非串流資源，轉譯期間 Direct3D 可防止特定危險條件，但因為危險追蹤是在串流資源的磚層級，串流資源轉譯期間的追蹤危險條件可能會太昂貴。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[建立串流資源](creating-streaming-resources.md)

 

 




