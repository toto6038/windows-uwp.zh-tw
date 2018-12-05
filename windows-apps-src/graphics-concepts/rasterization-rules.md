---
title: 點陣化規則
description: 點陣化規則定義向量資料對應至點陣資料的方式。
ms.assetid: B604725F-96A5-4DB6-B597-9EC57FBBC024
keywords:
- 點陣化規則
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c622c037f878d1ad34cdadf897dde10683532832
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8731609"
---
# <a name="rasterization-rules"></a>點陣化規則


點陣化規則定義向量資料對應至點陣資料的方式。 點陣資料會貼齊至整數位置，該整數位置接著會進行揀選和裁剪 (以繪製像素數目下限)，而每個像素屬性會在傳遞至像素著色器之前進行插入 (從每個頂點屬性)。

有數種規則根據將對應的基本類型，以及資料是否使用多重取樣來減少鋸齒化。 以下圖例示範異常情況的處理方式。

## <a name="span-idtrianglespanspan-idtrianglespanspan-idtrianglespantriangle-rasterization-rules-without-multisampling"></a><span id="Triangle"></span><span id="triangle"></span><span id="TRIANGLE"></span>三角形點陣化規則 (不含多重取樣)


繪製落在三角形內的任何像素中心；如果像素通過左上規則便假設是在內部。 左上規則是像素中心被定義為若位在三角形的上緣或左緣即落在三角形內。

其中：

-   上緣是完全水平邊緣並且在其他邊緣上方。
-   左緣是不完全水平且在三角形左側的邊緣，三角形可以有一個或兩個左緣。

左上規則確保只繪製相鄰三角形一次。

這個圖例顯示像素範例，繪製像素是因為它們不是落在三角形內，就是遵循左上規則。

![左上三角形點陣化的範例](images/d3d10-rasterrulestriangle.png)

淺灰和暗灰格內的像素顯示它們為像素群組，以指出它們在哪個三角形內。

## <a name="span-idline1spanspan-idline1spanspan-idline1spanline-rasterization-rules-aliased-without-multisampling"></a><span id="Line_1"></span><span id="line_1"></span><span id="LINE_1"></span>線條點陣化規則 (鋸齒化，沒有多重取樣)


線條點陣化規則使用菱形測試區域來判斷線條是否涵蓋像素。 若是 x 主要線條 (-1 &lt;= 斜率 &lt;= + 1 的線條)，菱形測試區域包括 (實線顯示) 左下緣、右下緣和底部角落；菱形排除 (點線顯示) 左上緣、右上緣、上方角落、左角落和右角落。 y 主要線條是非 x 主要線條的任何線條；測試菱形區與針對 x 主要線條所述的相同，只是除了右角落也包含在內以外。

提供菱形區域，當沿著線條從開頭到結束移動時若線條存在像素的菱形測試區域，則線條涵蓋像素。 帶狀線的行為相同，它是以一連串的線條繪製。

以下圖例顯示一些範例。

![鋸齒的線條點陣化範例](images/d3d10-rasterrulesline.png)

## <a name="span-idline2spanspan-idline2spanspan-idline2spanline-rasterization-rules-antialiased-without-multisampling"></a><span id="Line_2"></span><span id="line_2"></span><span id="LINE_2"></span>線條點陣化規則 (消除鋸齒，沒有多重取樣)


消除鋸齒的線條點陣化為如同它是矩形一般 (寬度 = 1)。 矩形會與產生每個像素涵蓋值的呈現目標相交，乘以該值成為像素著色器輸出 Alpha 元件。 在多重取樣的呈現目標上繪製線條時，不執行任何鋸齒化。

它被視為沒有單一最佳途徑來執行消除鋸齒線條呈現。 Direct3D 採用為如下圖所示方法的指導方針。 這個方法是憑經驗證明衍生而來的，展示數個被認為需要的視覺屬性。

硬體不需要與這個演算法完全相符；對此參考的測試應有「合理」容錯，並由以下進一步列出的原則引導，允許各種硬體實作及篩選核心大小。 硬體實作中不允許此彈性，但是可以透過 Direct3D 與應用程式進行通訊，只要繪製線條並觀察/測量它們的外觀。

![消除鋸齒的線條點陣化範例](images/d3d10-rasterruleslineaa.png)

這個演算法產生相對平滑的線條，具有一致的濃度，最低限度的鋸齒邊緣或編織。 將封閉線段的莫列波紋減至最小。 點對點放置的線段間的接合有良好的涵蓋。

篩選核心是邊緣模糊量和 Gamma 修正造成的濃度變更之間的合理取捨。 涵蓋值是按照以下公式乘上[輸出合併 (OM) 階段](output-merger-stage--om-.md)成為像素著色器 o0.a (srcAlpha)：

srcColor \* srcAlpha + destColor \* (1-srcAlpha)

## <a name="span-idpointspanspan-idpointspanspan-idpointspanpoint-rasterization-rules-without-multisampling"></a><span id="Point"></span><span id="point"></span><span id="POINT"></span>點點陣化規則 (沒有多重取樣)


點被解譯為如同由 Z 模式中的兩個三角形組成，而該三角形使用三角形點陣化規則。 座標會辨識一個像素寬的方形的中心。 沒有揀選任何點。

以下圖例顯示一些範例。

![點點陣化的範例](images/d3d10-rasterrulespoint.png)

## <a name="span-idmultisamplespanspan-idmultisamplespanspan-idmultisamplespanmultisample-anti-aliasing-rasterization-rules"></a><span id="Multisample"></span><span id="multisample"></span><span id="MULTISAMPLE"></span>多重取樣取消鋸齒點陣化規則


多重取樣取消鋸齒 (MSAA) 在多個子樣本位置，使用像素涵蓋範圍和深度樣板測試，來降低幾何鋸齒化。 為改善效能，像素計算是透過在所涵蓋子像素共用著色器輸出，為每一個涵蓋的像素執行一次。 多重取樣鋸齒化並不會減少表面鋸齒化。 樣本位置和重建功能是依硬體實作而定。

以下圖例顯示一些範例。

![多重取樣鋸齒化點陣化的範例](images/d3d10-rasterrulesmsaa.png)

樣本位置的數量依多重取樣模式而定。 頂點屬性會插入像素中心，因為這是叫用像素著色器的地方 (若未覆蓋中心，這會變成外插法)。 屬性可以在要距心取樣的像素著色器標幟，使非涵蓋的像素在像素區域和基本類型的交集處插入屬性。

像素著色器會為每一個 2x2 像素區域執行，以支援衍生性計算 (使用 x 和 y 差異)。 這表示著色器叫用的發生多於所顯示的，以填入最少 2x2 配量 (這不受多重取樣的影響)。 著色器結果會為每個覆蓋的且通過每個樣本深度樣板測試的樣本寫出。

基本類型的點陣化規則一般按照多重取樣鋸齒化保持不變，除了︰

-   對於三角形，會為每個樣本位置 (而不是像素中心) 執行涵蓋範圍測試。 如果涵蓋多個樣本位置，像素著色器會執行一次，在像素中央插入屬性。 為通過深度/樣板測試的像素中的每個涵蓋的樣本位置，儲存 (複製) 結果。

    線條會被視為兩個三角形組成的矩形，而線條的寬度為 1.4。

-   對於點，會為每個樣本位置 (而不是像素中心) 執行涵蓋範圍測試。

多重取樣格式可用於呈現目標，目標則可使用[載入](https://msdn.microsoft.com/library/windows/desktop/bb509694)讀回著色器，因為著色器所存取的個別樣本不需要任何解析。 多重取樣資源不支援深度格式，因此，深度格式只限於呈現目標。

無類型格式支援多重取樣，以允許資源檢視來解譯不同方式中的資料。 例如，您可以使用 R8G8B8A8\_TYPELESS 建立多重取樣資源，使用格式為 R8G8B8A8\_UINT 的呈現目標檢視資源呈現，然後將內容解析到另一個資料格式為 R8G8B8A8\_UNORM 的資源。

### <a name="span-idhardwaresupportspanspan-idhardwaresupportspanspan-idhardwaresupportspanhardware-support"></a><span id="Hardware_Support"></span><span id="hardware_support"></span><span id="HARDWARE_SUPPORT"></span>硬體支援

API 透過幾個品質等級報告對於多重取樣的硬體支援。 例如，0 的品質等級表示硬體不支援多重取樣 (在特定的格式及品質等級)。 3 的品質等級表示硬體支援三種不同樣本配置和/或解析演算法。 您也可以假設下列情況：

-   所有支援多重取樣的格式，支援該系列中每個格式相同數量的品質等級。
-   每個支援多重取樣的格式，並具有 \_UNORM、\_SRGB、\_SNORM 或 \_FLOAT 格式，也支援解析。

### <a name="span-idcentroidsamplingspanspan-idcentroidsamplingspanspan-idcentroidsamplingspancentroid-sampling-of-attributes-when-multisample-antialiasing"></a><span id="Centroid_Sampling"></span><span id="centroid_sampling"></span><span id="CENTROID_SAMPLING"></span>多重取樣鋸齒化時屬性的距心取樣

根據預設，頂點屬性會在多重取樣鋸齒化期間插入像素的中心；如果未涵蓋像素中心，屬性會外插到像素中心。 如果包含距心像素語意 (假設像素未完全涵蓋) 的像素著色器輸入，將在像素涵蓋區內的某個位置取樣，就可能是在涵蓋的樣本位置之一。 樣本遮罩 (由轉譯器狀態指定) 會在距心運算之前套用。 因此，遮罩住的樣本將不會用來做為距心位置。

參考轉譯器會選擇距心取樣的樣本位置，類似如下所列︰

-   樣本遮罩允許所有樣本。 如果涵蓋像素或未涵蓋任何樣本，則使用像素中心。 否則，選擇第一個涵蓋的樣本，從像素中心開始並向外移動。
-   樣本遮罩會關閉所有樣本，但其中一個 (常見案例) 除外。 應用程式可以實作多次超級取樣，方法是對於每個使用距心取樣的樣本，循環單一位元樣本遮罩值和重新呈現場景。 這需要應用程式調整導數，以適當選取更詳細的紋理 Mips，擁有較高的紋理取樣密度 。

### <a name="span-idderivativecalculationsspanspan-idderivativecalculationsspanspan-idderivativecalculationsspanderivative-calculations-when-multisampling"></a><span id="Derivative_Calculations"></span><span id="derivative_calculations"></span><span id="DERIVATIVE_CALCULATIONS"></span>多重取樣時的導數計算

像素著色器永遠使用最低的 2x2 像素區域來執行，以支援導數計算，其透過從相鄰像素的資料間得出差異來計算 (假設每個像素中的資料已取樣，以水平或垂直的單位間距取樣)。 這不受多重取樣的影響。

如果已經距心取樣的屬性要求導數，就不調整硬體計算，這可能導致不正確的導數。 著色器會預期呈現目標空間中的單位向量，但可能取得與一些其他向量空間相關的非單位向量。 因此，由應用程式負責在從已距心取樣的屬性要求導數時顯示警告。

事實上，建議不要結合導數和距心取樣。 距心取樣在這些情況可能很實用：基本類型的插入屬性不外插，但隨之而來的是要有所取捨，例如基本類型邊緣與像素交錯時跳動 (而不是持續變更) 或紋理取樣作業無法使用的導數衍生 LOD。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[附錄](appendix.md)

[轉譯器 (RS) 階段](rasterizer-stage--rs-.md)

 

 




