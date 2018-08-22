---
author: normesta
description: 地圖樣式表的項目和屬性
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 地圖樣式表參考
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 地圖, 地圖樣式表
ms.localizationpriority: medium
ms.openlocfilehash: 984741de5be585f7d6d726ec4c736e6ebce78830
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2801565"
---
# <a name="map-style-sheet-reference"></a>地圖樣式表參考

Microsoft 對應技術使用對應樣式表定義對應的外觀。  地圖樣式表定義使用 JavaScript Object Notation (JSON) 且可以用於各種方式包括在 Windows 市集應用程式的[MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)透過[MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_)方法。

例如，您想使用下列 JSON 讓水面區域顯示為紅色、水面標籤顯示為綠色，及陸地區域顯示為藍色：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
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

<a id="entries" />

## <a name="entries"></a>項目
此表格使用 ">" 字元代表項目階層層級。  它也會顯示 Windows 版本支援每個項目和其忽略。

| 組建 | Windows 版本名稱 |
|-------|----------------------|
| 1506  | Creators Update      |
| 1629  | Fall Creators Update |
| 1713  | 2018 年 4 月更新    |

| 名稱                         | 屬性群組            | 1506 | 1629 | 1713 | 下一則 | 說明    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| 版本                      | [版本](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | 您想要使用的樣式表版本。 |
| 設定                     | [設定](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | 適用於整個樣式表的設定。 |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有地圖項目的父項目。 |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有非使用者項目的父項目。 |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 使用描述園地的區域。  這些應該不至與此結構項目都實體建築物混淆。 |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含機場的區域。 |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 有高度集中的企業或興趣點的區域。 |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含 cemeteries 的區域。 |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 大陸區域標籤。 |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含學校及其他教育設施的區域。 |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含原住民人員會保留的區域。 |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 使用業界用途的區域。 |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 島區域標籤。 |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 進行醫療之用的區域 (例如： 醫院校園)。 |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含軍事型或有軍事用途的區域。 |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 進行航海用相關之用的區域。 |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 上的芳鄰] 區域中標籤。 |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 會做為紙飛機跑道的區域。 |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 沙地區域，如沙灘。 |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 為商場或其他購物中心配置的地面區域。 |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含後院的區域。 |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 地下區域 (例如：設置地鐵站)。 |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 森林、農田區域等。 |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 森林土地的區域。 |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含高爾夫課程的區域。 |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含公園的區域。 |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 圍出的場地，例如棒球場或網球場。 |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含性質的區域會保留。 |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 所有指向繪製的一些排序圖示的功能。 |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | 地址數字標籤。 |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表自然功能的圖示。 |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表山峰的圖示。 |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表火山山峰的圖示。 |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表水體位置的圖示，例如瀑布。 |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表有趣的任何位置的圖示。 |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表任何商務 locaiton 的圖示。 |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表旅遊資訊例如博物館、 zoos 等的圖示。 |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表位置的一般使用社群的圖示。 |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表學校和其他教育圖示相關位置。 |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表例如劇場、 cinemas 等娛樂 [地點的圖示。 |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表基本服務，例如駐留、 銀行、 ga 等的圖示。 |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表餐廳、 cafés 等的圖示。 |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表飯店及其他室企業版的圖示。 |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表 [實際空間企業版的圖示。 |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表飯店及其他室企業版的圖示。 |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表熱門地點規模的圖示 (例如︰城市或鎮)。 |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表熱門地點集中都市的圖示。 |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表州或省中首都的圖示。 |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表國家或地區首都的圖示。 |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表道路名稱縮寫的記號。 (例如：1-5)。 若您將設定項目的 **ImageFamily** 屬性設為*Palette*的值，則只能使用調色盤值 |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表出口的圖示，通常來自高速公路。 |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表公車站、火車站、機場等等的圖示。 |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 政治地區，例如國家、地區及縣市。 |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 國家 （地區） 框線及標籤。 |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1、 狀態、 省、 等 borders 和標籤。 |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2、 郡、 等 borders 和標籤。 |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建築物及其他類似建築物的結構。 |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建築物。 |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 使用適用於教育界的建築物。 |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建築物例如醫院醫療用途。 |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用來傳輸例如機場建築物。 |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 屬於運輸網路的線 (例如：道路、火車及渡輪)。 |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表所有道路的線。 |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表大型、 控制存取高速公路的簽名欄。 |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表通常是連線至的高速度坡道提升的行控制存取高速公路。 |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表高速公路的簽名欄。 |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表主要道路的簽名欄。 |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表 arterial 道路的簽名欄。 |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表街道的簽名欄。 |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表通常是連線至高速公路坡道提升的簽名欄。 |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表 unpaved 的街道的簽名欄。 |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表 [成本使用 money 道路的簽名欄。 |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 鐵路線。 |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 穿過公園或健行步道的步道。 |
| >>> 通訊組織走道                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 較高的通訊組織走道。 |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 渡船路徑線。 |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 任何像水的物體。 這包括海洋及溪流。 |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 河流、溪流或其他水道。  請注意，這可能是線或多邊形，可能連接到非河流的水體。 |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有路由相關項目。 |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 路由行相關項目。 |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表主導路由的簽名欄。 |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 代表 scenic 主導路由的簽名欄。 |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表從查閱路由行。 |
| > userMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有使用者項目。 |
| >> userBillboard             | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 預設 [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 執行個體的樣式。 |
| >> userLine                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 預設 [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) 執行個體的樣式。 |
| >> userModel3D               | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   | 預設 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 執行個體的樣式。  這主要是用於設定 renderAsSurface。 |
| >> userPoint                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 預設 [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) 執行個體的樣式。 |

<a id="properties" />

## <a name="properties"></a>內容

本節說明可用於各個項目的屬性。

<a id="version" />

### <a name="version-properties"></a>版本屬性

| 屬性                     | 類型    | 說明                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| 版本                      | 字串  | 目標式樣式表版本。 用於適用性。 「1.0」為預設值「1.*」為其他次要功能更新。 |

<a id="settings" />

### <a name="settings-properties"></a>設定屬性

| 屬性                     | 類型    | 1506 | 1629 | 1713 | 下一則 | 說明 |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，表示氣象是否出現在 3D 控制項中。 |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   | 旗標，表示是否顯示具有紋理的符號 3D 建築的紋理。 |
| fogColor                     | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 出現在 3D 控制項的距離模糊 ARGB 色彩值。 |
| glowColor                    | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 可能會套用到標籤光暈與圖示光暈的 ARGB 色彩值。 |
| imageFamily                  | 字串  |  ✔   |  ✔   |  ✔   |  ✔   | 用於此樣式的影像名稱 針對使用以真實世界標誌為依據之固定色彩的記號，將此值設為*Default*。 針對使用調色盤可調式色彩的記號，將這個值設為*Palette*。 |
| landColor                    | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 在土地上未繪製任何項目前，土地的 ARGB 色彩值。 |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，表示具有**Organization**屬性的項目是否應繪製適當的標誌或使用通用圖標。 |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，表示具有官方色彩屬性的項目（例如中國的中轉線）應繪製該色彩。 例如，關閉此值即為黑白地圖。 |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 會指出要繪製點陣區域其中有較佳的表示法比向量 （日本和韓國） 旗標。 |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，指出是否在地圖上繪製海拔著色。 |
| shadedReliefDarkColor        | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 暈渲地貌的暗面色彩。  Alpha 通道代表最大 alpha 值。 |
| shadedReliefLightColor       | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 暈渲地貌的亮面色彩。  Alpha 通道代表最大 alpha 值。 |
| shadowColor                  | 色彩   |      |      |      |  ✔️   | 使用陰影的圖示背後的陰影色彩。 |
| spaceColor                   | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 地圖周圍區域的 ARGB 色彩值。 |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 指出是否在 SVG 原來的色彩應使用，而不是要找的映像中的色彩調色盤項目向上旗標。 |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement 屬性

| 屬性                     | 類型    | 1506 | 1629 | 1713 | 下一則 | 描述 |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   | 應該縮放的圖示的背景元素量。  例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| fillColor                    | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 用來填滿多邊形、點圖示背景的色彩，如果線已分割，則用於填滿線的中央。 |
| fontFamily                   | 字串  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 顯示於點圖示中間的字符色彩。 |
| iconScale                    | 浮點數   |      |  ✔   |  ✔   |  ✔   | 應該縮放的字符的背景元素量。  例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| labelColor                   | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   | 預設標籤大小的調整量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 讓 **FillColor** 的 alpha 值覆寫 **StrokeColor** 而不是與之混合。 |
| scale                        | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   | 整個點的大小調整量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| strokeColor                  | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 要用於多邊形邊框、點圖示邊框的色彩，以及線條色彩。 |
| strokeWidthScale             | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   | 線筆觸的調整量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

這個屬性群組繼承自[MapElement](#mapelement)屬性群組。

| 屬性                     | 類型    | 1506 | 1629 | 1713 | 下一則 | 說明 |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 填滿的多邊形邊框的次要或外框線色彩。 |
| borderStrokeColor            | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 填滿的多邊形邊框的主要線色彩。 |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   | 要調整的框線筆劃所依據量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle 屬性

這個屬性群組繼承自[MapElement](#mapelement)屬性群組。

| 屬性                     | 類型    | 1506 | 1629 | 1713 | 下一則 | 描述 |
|------------------------------|---------|------|------|------|------|-------------|
| 圖形背景             | 浮點數   |      |      |      |  ✔️   | 若要使用做為圖示-取代存在於那里任何圖形的背景圖形。 |
| stemAnchorRadiusScale        | 浮點數   |      |      |  ✔   |  ✔   | 應該縮放的圖示主體的錨點量。  例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| stemColor                    | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 在 3D 模式下，圖示底部出現的主體色彩。 |
| stemHeightScale              | 浮點數   |      |      |  ✔   |  ✔   | 應該縮放的圖示主體的長度量。  例如，使用 *1* 顯示預設和 *2* 以加長二倍。 |
| stemOutlineColor             | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 在 3D 模式下，圖示底部出現的主體周圍邊框色彩。 |
| stemWidthScale               | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   | 應該縮放的圖示主體的寬度量。  例如，使用 *1* 顯示預設和 *2* 以加長二倍。 |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

這個屬性群組繼承自 [MapElement](#mapelement) 屬性群組。

| 屬性                     | 類型    | 1506 | 1629 | 1713 | 下一則 | 描述 |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   | 旗標，表示應轉譯的 3D 模型，例如建築，而無地面的深度淡出。 |
