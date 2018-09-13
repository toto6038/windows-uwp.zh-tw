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
ms.openlocfilehash: 11360f9d76fc07d7a6b24bd1e0bfb78df4f1d22d
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2018
ms.locfileid: "3957956"
---
# <a name="map-style-sheet-reference"></a>地圖樣式表參考

Microsoft 對應技術會使用_地圖樣式表_來定義地圖的外觀。  地圖樣式表使用 JavaScript 物件標記法 (JSON) 定義，並且可在各種不同的方式包含在 Windows 市集應用程式的[MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)透過[MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_)方法。

以互動方式使用[地圖樣式表編輯器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft)應用程式可以建立樣式表。

可用於下列 JSON 讓水面區域顯示為紅色、 水面標籤顯示為綠色，及陸地區域顯示為藍色：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

這個 JSON 可用來移除地圖中的所有標籤與點。

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
此表格使用 ">" 字元代表項目階層層級。  它也會顯示哪些版本的 Windows 支援的每個項目，而且這忽略它。

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
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 利用描述土地的區域。  這些不應該與結構項目底下的實體建築物混淆。 |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含機場的區域。 |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 有高度集中的企業或興趣點的區域。 |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含 cemeteries 的區域。 |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 將標籤大陸的區域。 |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含學校及其他教育功能的區域。 |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 保護包含原住民的區域。 |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 工業用途的區域。 |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 島區域中的標籤。 |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 提供醫療用途的區域 (例如︰ 醫院園區)。 |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含軍事基地或有 military 用途的區域。 |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 提供航海相關用途的區域。 |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 街區區域標籤。 |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 做為飛航陸地區域。 |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 沙地區域，如沙灘。 |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 為商場或其他購物中心配置的地面區域。 |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含後院的區域。 |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 地下區域 (例如：設置地鐵站)。 |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 森林、農田區域等。 |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 森林土地的區域。 |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含高爾夫課程的區域。 |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含公園區域。 |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 圍出的場地，例如棒球場或網球場。 |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 保護包含本質上的區域。 |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 所有點會繪製的某種圖示的功能。 |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | 地址號碼標籤。 |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表自然功能的圖示。 |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表山峰的圖示。 |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表火山山峰的圖示。 |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表水體位置的圖示，例如瀑布。 |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示有趣的任何位置的圖示。 |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表任何商務 locaiton 圖示。 |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表旅遊，例如博物館、 zoos 等等的圖示。 |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 社群代表位置的一般用途的圖示。 |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表學校及其他教育版的相關的位置。 |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表娛樂場合，例如劇場、 cinemas 等等的圖示。 |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表重要的服務，例如停車、 銀行、 ga 等等的圖示。 |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表餐廳、 咖啡廳等等的圖示。 |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表旅館和其他住宿企業的圖示。 |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表實際可用空間企業圖示。 |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表旅館和其他住宿企業的圖示。 |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表熱門地點規模的圖示 (例如︰城市或鎮)。 |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表熱門地點集中都市的圖示。 |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表州或省中首都的圖示。 |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表國家或地區首都的圖示。 |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表道路名稱縮寫的記號。 (例如：1-5)。 若您將設定項目的 **ImageFamily** 屬性設為*Palette*的值，則只能使用調色盤值 |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表出口的圖示，通常來自高速公路。 |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表公車站、火車站、機場等等的圖示。 |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 政治地區，例如國家、地區及縣市。 |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 國家/地區區域邊界和標籤。 |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1、 州、 省等框線和標籤。 |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2、 縣等等，框線和標籤。 |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建築物及其他類似建築物的結構。 |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建築物。 |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建築物教育版使用。 |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建築物醫院例如醫療用途。 |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用於運輸工具，例如機場建築物。 |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 屬於運輸網路的線 (例如：道路、火車及渡輪)。 |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表所有道路的線。 |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表高速公路大型的受控制存取的線。 |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表高速坡道，通常與它連線的控制存取高速公路。 |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表高速公路線。 |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表主要道路的線。 |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表 arterial 道路的線。 |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表街道線。 |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表坡道旁通常連線到高速公路線。 |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表 unpaved 的街道線。 |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表付費才能使用的道路的線。 |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 鐵路線。 |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 穿過公園或健行步道的步道。 |
| >>> 通訊組織走道                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 提升權限的通訊組織走道。 |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 渡船路徑線。 |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 任何像水的物體。 這包括海洋及溪流。 |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 河流、溪流或其他水道。  請注意，這可能是線或多邊形，可能連接到非河流的水體。 |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有路由相關項目。 |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 路由傳送行相關的項目。 |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表行駛路線的線。 |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 代表景區行駛路線的線。 |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表步行路線行。 |
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
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，指出要繪製點陣區域，其中有更好的表示法，比向量 （日本及韓國）。 |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，指出是否在地圖上繪製海拔著色。 |
| shadedReliefDarkColor        | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 暈渲地貌的暗面色彩。  Alpha 通道代表 alpha 的最大值。 |
| shadedReliefLightColor       | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 暈渲地貌的亮面色彩。  Alpha 通道代表 alpha 的最大值。 |
| shadowColor                  | 色彩   |      |      |      |  ✔️   | 使用陰影的圖示後方陰影的色彩。 |
| spaceColor                   | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   | 地圖周圍區域的 ARGB 色彩值。 |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，指出是否 SVG 內的原始色彩應該使用，而不是設定的影像中的色彩調色盤項目的外觀。 |

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
| borderWidthScale             | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   | 框線的筆觸的調整量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle 屬性

這個屬性群組繼承自[MapElement](#mapelement)屬性群組。

| 屬性                     | 類型    | 1506 | 1629 | 1713 | 下一則 | 描述 |
|------------------------------|---------|------|------|------|------|-------------|
| 圖形背景             | 浮點數   |      |      |      |  ✔️   | 做為圖示： 取代存在任何圖形的背景圖形。 |
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
