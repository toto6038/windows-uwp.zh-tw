---
author: normesta
description: "地圖樣式表的項目和屬性"
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "地圖樣式表參考"
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 地圖, 地圖樣式表"
ms.openlocfilehash: 9e2605917027d7be96ac86421e83082aa5f368f9
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/19/2017
---
# <a name="map-style-sheet-reference"></a>地圖樣式表參考

您可以使用 JavaScript 物件標記法 (JSON) 來建立地圖樣式表。

例如，您想使用下列 JSON 讓水面區域顯示為紅色、水面標籤顯示為綠色，及陸地區域顯示為藍色：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000", "labelColor":"#00FF00"}}
    }
```
您也可以使用 JSON 移除地圖中的所有標籤與點。

```json

    {"version":"1.*", "elements":{"mapElement":{"labelVisible":false},"point":{"visible":false}}}
```

有時候屬性的值會被轉換以產生最終結果。  例如，植被填滿色彩會根據所顯示的實體類型而有稍微不同的灰階。  可以使用 ignoreTransform 屬性關閉此行為，從而使用精確的提供值。

```json
    {"version":"1.*",
        "settings":{"shadedReliefVisible":false},
        "elements":{"vegetation":{"fillColor":{"value":"#999999","ignoreTransform":true}}}
    }
```

此主題顯示 JSON 項目和[屬性](#properties)，您可以用來自訂您地圖中的外觀及操作。

<span id="entries" />
## <a name="entries"></a>項目
此表格使用 ">" 字元代表項目階層層級。   

| 名稱                         | 屬性群組            | 說明    |
|------------------------------|---------------------------|----------------|
| 版本                      | [版本](#version)       | 您想要使用的樣式表版本。 |
| 設定                     | [設定](#settings)     | 適用於整個樣式表的設定。 |
| mapElement                   | [MapElement](#mapelement) | 所有地圖項目的父項目。 |
| > baseMapElement             | [MapElement](#mapelement) | 所有非使用者項目的父項目。 |
| >> area                      | [MapElement](#mapelement) | 土地利用區域 (不會與結構項目混淆)。 |
| >>> airport                  | [MapElement](#mapelement) | 機場周圍的區域。 |
| >>> cemetery                 | [MapElement](#mapelement) | 墓地區域。 |
| >>> continent                | [MapElement](#mapelement) | 整個大陸的區域。 |
| >>> education                | [MapElement](#mapelement) |  |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  |
| >>> island                   | [MapElement](#mapelement) | 島區域中的標籤。 |
| >>> medical                  | [MapElement](#mapelement) | 提供醫療用途的土地區域 (例如︰醫院園區)。 |
| >>> military                 | [MapElement](#mapelement) | 軍事基地的區域。 |
| >>> nautical                 | [MapElement](#mapelement) | 提供航海用途的土地區域。 |
| >>> neighborhood             | [MapElement](#mapelement) | 定義為鄰近地區區域中的標籤。 |
| >>> runway                   | [MapElement](#mapelement) | 跑道覆蓋的陸地區域。 |
| >>> sand                     | [MapElement](#mapelement) | 沙地區域，如沙灘。 |
| >>> shoppingCenter           | [MapElement](#mapelement) | 為商場或其他購物中心配置的地面區域。 |
| >>> stadium                  | [MapElement](#mapelement) | 體育場的區域。 |
| >>> vegetation               | [MapElement](#mapelement) | 森林、農田區域等。 |
| >>>> forest                  | [MapElement](#mapelement) | 森林土地的區域。 |
| >>>> golfCourse              | [MapElement](#mapelement) |  |
| >>>> park                    | [MapElement](#mapelement) | 公園區域。 |
| >>>> playingField            | [MapElement](#mapelement) | 圍出的場地，例如棒球場或網球場。 |
| >>>> reserve                 | [MapElement](#mapelement) | 大自然保護區域。 |
| >> point                     | [PointStyle](#pointstyle) | 所有以某種圖示轉譯的點狀地物。 |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  |
| >>>> peak                    | [PointStyle](#pointstyle) | 代表山峰的圖示。 |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) | 代表火山山峰的圖示。 |
| >>>> waterPoint              | [PointStyle](#pointstyle) | 代表水體位置的圖示，例如瀑布。 |
| >>> pointOfInterest          | [PointStyle](#pointstyle) | 餐廳、醫院、學校、船塢、滑雪區域等等。 |
| >>>> business                | [PointStyle](#pointstyle) | 餐廳、醫院、學校等。 |
| >>>>> foodPoint              | [PointStyle](#pointstyle) | 餐廳、咖啡廳等等。 |
| >>> populatedPlace           | [PointStyle](#pointstyle) | 代表熱門地點規模的圖示 (例如︰城市或鎮)。 |
| >>>> capital                 | [PointStyle](#pointstyle) | 代表熱門地點集中都市的圖示。 |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) | 代表州或省中首都的圖示。 |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) | 代表國家或地區首都的圖示。 |
| >>> roadShield               | [PointStyle](#pointstyle) | 代表道路名稱縮寫的記號。 (例如：1-5)。 若您將設定項目的 **ImageFamily** 屬性設為*Palette*的值，則只能使用調色盤值 |
| >>> roadExit                 | [PointStyle](#pointstyle) | 代表出口的圖示，通常來自高速公路。 |
| >>> transit                  | [PointStyle](#pointstyle) | 代表公車站、火車站、機場等等的圖示。 |
| >> political                 | [BorderedMapElement](#borderedmapelement) | 政治地區，例如國家、地區及縣市。 |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) | Admin1、州、省等。 |
| >>> district                 | [BorderedMapElement](#borderedmapelement) | Admin2、縣等等。 |
| >> structure                 | [MapElement](#mapelement) | 建築物及其他類似建築物的結構。 |
| >>> building                 | [MapElement](#mapelement) |  |
| >>>> educationBuilding       | [MapElement](#mapelement) |  |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  |
| >>>> transitBuilding         | [MapElement](#mapelement) |  |
| >> transportation            | [TwoToneLineStyle](#twotonelinestyle) | 屬於運輸網路的線(例如：道路、火車及渡輪)。 |
| >>> road                     | [TwoToneLineStyle](#twotonelinestyle) | 代表所有道路的線。 |
| >>>> controlledAccessHighway | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>>> highSpeedRamp          | [TwoToneLineStyle](#twotonelinestyle) | 代表坡道的線。 這些坡道旁通常出現高速公路兩旁。 |
| >>>> highway                 | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> majorRoad               | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> arterialRoad            | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> street                  | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>>> ramp                   | [TwoToneLineStyle](#twotonelinestyle) | 代表高速公路入口和出口的線。 |
| >>>>> unpavedStreet          | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> tollRoad                | [TwoToneLineStyle](#twotonelinestyle) | 須付費才能使用的道路。 |
| >>> railway                  | [TwoToneLineStyle](#twotonelinestyle) | 鐵路線。 |
| >>> trail                    | [TwoToneLineStyle](#twotonelinestyle) | 穿過公園或健行步道的步道。 |
| >>> waterRoute               | [TwoToneLineStyle](#twotonelinestyle) | 渡船路徑線。 |
| >> water                     | [MapElement](#mapelement) | 任何像水的物體。 這包括海洋及溪流。 |
| >>> river                    | [MapElement](#mapelement) | 河流、溪流或其他水道。  請注意，這可能是線或多邊形，可能連接到非河流的水體。 |
| > routeMapElement            | [MapElement](#mapelement) | 所有路徑條目都在此項目下。 |
| >> routeLine                 | [TwoToneLineStyle](#twotonelinestyle) | 所有路徑線的樣式。 |
| >>> walkingRoute             | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>> drivingRoute             | [TwoToneLineStyle](#twotonelinestyle) |  |
| > userMapElement             | [MapElement](#mapelement) | 所有使用者項目都在此項目下。 |
| >> userPoint                 | [PointStyle](#pointstyle) | 預設使用者點的樣式。 |
| >> userLine                  | [MapElement](#mapelement) | 預設使用者線的樣式。 |

<span id="properties" />
## <a name="properties"></a>屬性

本節說明可用於各個項目的屬性。

<span id="version" />
### <a name="version-properties"></a>版本屬性

| 屬性                     | 類型    | 說明                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| 版本                      | 字串  | 目標式樣式表版本。 用於適用性。 「1.0」為預設值「1.*」為其他次要功能更新。 |

<span id="settings" />
### <a name="settings-properties"></a>設定屬性

| 屬性                     | 類型    | 說明                                                                                                                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| atmosphereVisible            | Bool    | 旗標，表示氣象是否出現在 3D 控制項中。                                                                                                                                                     |
| fogColor                     | 色彩   | 出現在 3D 控制項的距離模糊 ARGB 色彩值。                                                                                                                                                    |
| glowColor                    | 色彩   | 可能會套用到標籤光暈與圖示光暈的 ARGB 色彩值。                                                                                                                                                     |
| imageFamily                  | 字串  | 用於此樣式的影像名稱 針對使用以真實世界標誌為依據之固定色彩的記號，將此值設為*Default*。 針對使用調色盤可調式色彩的記號，將這個值設為*Palette*。 |
| landColor                    | 色彩   | 在土地上未繪製任何項目前，土地的 ARGB 色彩值。                                                                                                                                                     |
| logosVisible                 | Bool    | 旗標，表示具有**Organization**屬性的項目是否應繪製適當的標誌或使用通用圖標。                                                                                         |
| officialColorVisible         | Bool    | 旗標，表示具有官方色彩屬性的項目（例如中國的中轉線）應繪製該色彩。 例如，關閉此值即為黑白地圖。                               |
| rasterRegionsVisible         | Bool    | 旗標，指出是否要在不依向量轉譯處繪製點陣區域(例如︰日本及韓國)。                                                                                                |
| shadedReliefVisible          | Bool    | 旗標，指出是否在地圖上繪製海拔著色。                                                                                                                                                  |
| shadedReliefDarkColor        | 色彩   | 暈渲地貌的暗面色彩。  Alpha 通道代表 alpha上限。 值。                                                                                                                            |
| shadedReliefLightColor       | 色彩   | 暈渲地貌的亮面色彩。  Alpha 通道代表 alpha上限。 值。                                                                                                                           |
| spaceColor                   | 色彩   | 地圖周圍區域的 ARGB 色彩值。                                                                                                                                                                               |
| useDefaultImageColors        | Bool    | 旗標，表示是否應使用SVG中的原始顏色，而不是為影像中色彩查找調色盤項目。                                                                                |

<span id="mapelement" />
### <a name="mapelement-properties"></a>MapElement 屬性

| 屬性                     | 類型    | 說明                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------|
| fillColor                    | 色彩   | 用來填滿多邊形、點圖示背景的色彩，如果線已分割，則用於填滿線的中央。 |
| fontFamily                   | 字串  |                                                                                                                             |
| labelColor                   | 色彩   |                                                                                                                             |
| labelOutlineColor            | 色彩   |                                                                                                                             |
| labelScale                   | 浮點數   | 預設標籤大小的調整量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。            |
| labelVisible                 | Bool    |                                                                                                                             |
| strokeColor                  | 色彩   | 要用於多邊形邊框、點圖示邊框的色彩，以及線條色彩。                   |
| strokeWidthScale             | 浮點數   | 線筆觸的調整量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。            |
| visible                      | Bool    |                                                                                                                             |

<span id="borderedmap" />
### <a name="borderedmapelement"></a>BorderedMapElement

這個屬性群組繼承自[MapElement](#mapelement)屬性群組。

| 屬性                     | 類型    | 說明                                                           |
|------------------------------|---------|-----------------------------------------------------------------------|
| borderOutlineColor           | 色彩   | 填滿的多邊形邊框的次要或外框線色彩。 |
| borderStrokeColor            | 色彩   | 填滿的多邊形邊框的主要線色彩。             |
| borderVisible                | Bool    |                                                                       |
| borderWidthScale             | 浮點數   |                                                                       |

<span id="pointstyle" />
### <a name="pointstyle-properties"></a>PointStyle 屬性

這個屬性群組繼承自[MapElement](#mapelement)屬性群組。

| 屬性                     | 類型    | 說明                                                                                                        |
|------------------------------|---------|--------------------------------------------------------------------------------------------------------------------|
| iconColor                    | 色彩   | 顯示於點圖示中間的字符色彩。                                                        |
| scale                        | 浮點數   | 整個點的大小調整量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| stemColor                    | 色彩   | 在 3D 模式下，圖示底部出現的主體色彩。                                             |
| stemOutlineColor             | 色彩   | 在 3D 模式下，圖示底部出現的主體周圍邊框色彩。                          |

<span id="twotonelinestyle" />
### <a name="twotonelinestyle-properties"></a>TwoToneLineStyle 屬性

這個屬性群組繼承自[MapElement](#mapelement)屬性群組。

| 屬性                     | 類型    | 說明                                                                                          |
|------------------------------|---------|------------------------------------------------------------------------------------------------------|
| overwriteColor               | Bool    |  讓**FillColor** 的 alpha 值覆寫 **StrokeColor** 而不是與之混合。 |
