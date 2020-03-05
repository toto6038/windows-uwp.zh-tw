---
description: 地圖樣式表的項目和屬性
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 地圖樣式表參考
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, uwp, 地圖, 地圖樣式表
ms.localizationpriority: medium
ms.openlocfilehash: b59e8c3c6d9c4c299e441964be1afb4e02051e23
ms.sourcegitcommit: 5264d7499ddbe21199a63d74a294206069f90f8b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2020
ms.locfileid: "78287449"
---
# <a name="map-style-sheet-reference"></a>地圖樣式表參考

Microsoft 對應技術會使用_地圖樣式表單_來定義地圖的外觀。  地圖樣式表單是使用 JavaScript 物件標記法（JSON）所定義，而且可以在 Windows Store 應用程式的[MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)中透過[MapStyleSheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_)方法來使用，包括在內的各種方式。

樣式表單可以使用 [[地圖樣式表單編輯器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft)] 應用程式以互動方式建立。

下列 JSON 可用來讓水區域以紅色顯示，浮水印會以綠色顯示，而土地區域則以藍色顯示：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

這個 JSON 可用來移除對應中的所有標籤和點。

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

此主題顯示 JSON 項目和[屬性](#properties)，您可以用來自訂您地圖中的外觀及操作。  這些屬性也可以透過[MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry)屬性套用至使用者的地圖元素。

<a id="entries" />

## <a name="entries"></a>項目
此表格使用 ">" 字元代表項目階層層級。  它也會顯示哪些版本的 Windows 支援每個專案，並忽略它。

| 版本 | Windows 版本名稱 |
|---------|----------------------|
|  1703   | Creators Update      |
|  1709   | Fall Creators Update |
|  1803   | 2018 年 4 月更新    |
|  1809   | 2018年10月更新  |
|  1903   | 5月2019更新      |

| 名稱                               | 內容群組            | 1703 | 1709 | 1803 | 1809 | 1903 | 描述    |
|------------------------------------|---------------------------|------|------|------|------|------|----------------|
| 版本                            | [版本](#version)       |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 您想要使用的樣式表版本。 |
| 設定                           | [設定](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 適用於整個樣式表的設定。 |
| mapElement                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 所有地圖項目的父項目。 |
| > baseMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 所有非使用者項目的父項目。 |
| >> area                            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 描述土地使用的區域。  這些不應該與結構專案下的實體大樓混淆。 |
| >>> airport                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含機場的區域。 |
| >>> areaOfInterest                 | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 有高度集中的企業或興趣點的區域。 |
| >>> cemetery                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含 cemeteries 的區域。 |
| >>> continent                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 大陸區域標籤。 |
| >>> education                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含學校和其他教育機構的區域。 |
| >>> indigenousPeoplesReserve       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含 indigenous 人的領域會保留。 |
| >>> industrial                     | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 用於工業用途的區域。 |
| >>> island                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 島區標籤。 |
| >>> medical                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用於醫療用途的區域（例如：醫院校園）。 |
| >>> military                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含軍事基底或具有軍事用途的區域。 |
| >>> nautical                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用於海裡相關用途的區域。 |
| >>> neighborhood                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 鄰近區域標籤。 |
| >>> runway                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用來做為飛機跑道的區域。 |
| >>> sand                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 沙地區域，如沙灘。 |
| >>> shoppingCenter                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 為商場或其他購物中心配置的地面區域。 |
| >>> stadium                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含運動場的區域。 |
| >>> underground                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 地下區域 (例如：設置地鐵站)。 |
| >>> vegetation                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 森林、農田區域等。 |
| >>>> forest                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 森林土地的區域。 |
| >>>> golfCourse                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含高爾夫球堂課程的區域。 |
| >>>> park                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含公園的區域。 |
| >>>> playingField                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 圍出的場地，例如棒球場或網球場。 |
| >>>> reserve                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含本質保留的區域。 |
| > > frozenWater                     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 凍結的水，例如 glacier。 |
| >> point                           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 以某種圖示繪製的所有點特徵。 |
| >>> address                        | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   |  ✔   | 位址號碼標籤。 |
| >>> naturalPoint                   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表自然功能的圖示。 |
| >>>> peak                          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表山峰的圖示。 |
| >>>>> volcanicPeak                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表火山山峰的圖示。 |
| >>>> waterPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表水體位置的圖示，例如瀑布。 |
| >>> pointOfInterest                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表任何有趣位置的圖示。 |
| >>>> business                      | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表任何商務位置的圖示。 |
| > > > > > attractionPoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表旅遊景點的圖示，例如博物館、動物園等等。 |
| > > > > > > amusementPlacePoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 娛樂位置圖示。 |
| > > > > > > aquariumPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 水族箱圖示。 |
| > > > > > > artGalleryPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 美工圖庫圖示。 |
| > > > > > > campPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Camp 圖示。 |
| > > > > > > fishingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 釣魚圖示。 |
| > > > > > > gardenPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 花園圖示。 |
| > > > > > > hikingPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 健行圖示。 |
| > > > > > > libraryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 程式庫圖示。 |
| > > > > > > museumPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 博物館圖示。 |
| > > > > > > naturalPlacePoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 自然位置圖示。 |
| > > > > > > parkPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 公園圖示。 |
| > > > > > > restAreaPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Rest 區域圖示。 |
| > > > > > > touristAttractionPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 旅遊遊樂設施圖示。 |
| > > > > > > zooPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Zoo 圖示。 |
| > > > > > communityPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 圖示，代表對該社區的一般使用位置。 |
| > > > > > > conventionCenterPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 慣例中心圖示。 |
| > > > > > > financialPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 財務圖示。 |
| > > > > > > governmentPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 政府圖示。 |
| > > > > > > informationTechnologyPoint  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 資訊技術圖示。 |
| > > > > > > palacePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Palace 圖示。 |
| > > > > > > pollingStationPoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 輪詢站圖示。 |
| > > > > > > researchPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 研究圖示。 |
| > > > > > educationPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表學校和其他教育相關位置的圖示。 |
| > > > > > entertainmentPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表娛樂地點的圖示，例如劇院、cinemas 等等。 |
| > > > > > > artsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 藝術圖示。 |
| > > > > > > bowlingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 保齡球圖示。 |
| > > > > > > casinoPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 賭場圖示。 |
| > > > > > > golfCoursePoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 高爾夫課程圖示。 |
| > > > > > > gymPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Gym 圖示。 |
| > > > > > > marinaPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 港灣圖示。 |
| > > > > > > movieTheaterPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Movie 劇院圖示。 |
| > > > > > > nightClubPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 夜晚俱樂部圖示。 |
| > > > > > > recreationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 重新的圖示。 |
| > > > > > > skatingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 溜冰圖示。 |
| > > > > > > skiAreaPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Ski 區域圖示。 |
| > > > > > > stadiumPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Swimming 集區圖示。 |
| > > > > > > swimmingPoolPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Swimming 集區圖示。 |
| > > > > > > theaterPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 劇院圖示。 |
| > > > > > > wineryPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Winery 圖示。 |
| > > > > > essentialServicePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表重要服務的圖示，例如停車、銀行、天然氣等等。 |
| > > > > > > aTMPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ATM 圖示。 |
| > > > > > > automobileRentalPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 汽車出租圖示。 |
| > > > > > > automobileRepairPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 汽車修復圖示。 |
| > > > > > > bankPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 銀行圖示。 |
| > > > > > > clinicPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 實務圖示。 |
| > > > > > > electricChargingStationPoint| [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 電力充電站圖示。 |
| > > > > > > fireStationPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | FireStation 圖示。 |
| > > > > > > gasStationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | GasStation 圖示。 |
| > > > > > > groceryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 雜貨圖示。 |
| > > > > > > hospitalPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 醫院圖示。 |
| > > > > > > laundryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 總圖示。 |
| > > > > > > liquorAndWineStorePoint     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 去搶劫便利和葡萄酒店圖示。 |
| > > > > > > mailPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 郵件圖示。 |
| > > > > > > marketPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 市場圖示。 |
| > > > > > > parkingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 停車圖示。 |
| > > > > > > petsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 寵物圖示。 |
| > > > > > > pharmacyPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 藥房圖示。 |
| > > > > > > policePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 員警圖示。 |
| > > > > > > postalServicePoint          | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 郵政服務圖示。 |
| > > > > > > professionalPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 專業服務圖示。 |
| > > > > > > toiletPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 抽水馬桶圖示。 |
| > > > > > > veterinarianPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 獸醫圖示。 |
| >>>>> foodPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表餐廳、網咖等的圖示。 |
| > > > > > > barAndGrillPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 橫條和燒烤圖示。 |
| > > > > > > barPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 橫條圖示。 |
| > > > > > > breweryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Brewery 圖示。 |
| > > > > > > cafePoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 咖啡廳圖示。 |
| > > > > > > iceCreamShopPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 霜淇淋店圖示。 |
| > > > > > > restaurantPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 餐廳圖示。 |
| > > > > > > teaShopPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | TeaShop 圖示。 |
| > > > > > lodgingPoint                 | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表飯店與其他住宿企業的圖示。 |
| > > > > > > gotelPoint                  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 飯店圖示。 |
| > > > > > realEstatePoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表房地產企業的圖示。 |
| > > > > > shoppingPoint                | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表飯店與其他住宿企業的圖示。 |
| > > > > > > automobileDealerPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 汽車轉銷商圖示。 |
| > > > > > > beautyAndSpaPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 美和 spa 圖示。 |
| > > > > > > bookstorePoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 書店圖示。 |
| > > > > > > departmentStorePoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 部門商店圖示。 |
| > > > > > > electronicsShopPoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 電子產品商店圖示。 |
| > > > > > > golfShopPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 高爾夫球店圖示。 |
| > > > > > > homeApplianceShopPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 家用設備購買圖示。 |
| > > > > > > mallPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 購物中心圖示。 |
| > > > > > > phoneShopPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 電話店面圖示。 |
| >>> populatedPlace                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表熱門地點規模的圖示 (例如︰城市或鎮)。 |
| >>>> capital                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表熱門地點集中都市的圖示。 |
| >>>>> adminDistrictCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表州或省中首都的圖示。 |
| > > > > > > majorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 代表州或省之主要資本的圖示。 |
| > > > > > > minorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 代表州或省之次要資本的圖示。 |
| >>>>> countryRegionCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表國家或地區首都的圖示。 |
| > > > > majorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 代表主要已填入位置之大小的圖示。 |
| > > > > minorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 代表次要填入位置大小的圖示。 |
| >>> roadShield                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表道路名稱縮寫的記號。 (例如：1-5)。 若您將設定項目的 **ImageFamily** 屬性設為*Palette*的值，則只能使用調色盤值 |
| >>> roadExit                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表出口的圖示，通常來自高速公路。 |
| >>> transit                        | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表公車站、火車站、機場等等的圖示。 |
| >> political                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 政治地區，例如國家、地區及縣市。 |
| >>> countryRegion                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 國家/地區的邊界和標籤。 |
| >>> adminDistrict                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin1、州、省等等、框線和標籤。 |
| >>> district                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin2、縣/市等等、框線和標籤。 |
| >> structure                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 建築物及其他類似建築物的結構。 |
| >>> building                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 建築物. |
| >>>> educationBuilding             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用於教育的大樓。 |
| >>>> medicalBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用於醫療用途的大樓，例如醫院。 |
| >>>> transitBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用於運輸的大樓，例如機場。 |
| >> transportation                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 屬於運輸網路的線 (例如：道路、火車及渡輪)。 |
| >>> road                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表所有道路的線。 |
| >>>> controlledAccessHighway       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表大型、受控制存取權高速公路的線條。 |
| >>>>> highSpeedRamp                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表高速斜坡的線條，通常會連接到受控制的存取高速公路。 |
| >>>> highway                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表高速公路的線條。 |
| >>>> majorRoad                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表主要道路的線條。 |
| >>>> arterialRoad                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表 arterial 道路的線條。 |
| >>>> street                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表街道的線條。 |
| >>>>> ramp                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表通常會連接到高速公路之斜坡的線條。 |
| >>>>> unpavedStreet                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表 unpaved 街道的線條。 |
| >>>> tollRoad                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 這幾行代表要使用成本效益的道路。 |
| >>> railway                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 鐵路線。 |
| >>> trail                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 穿過公園或健行步道的步道。 |
| > > > walkway                        | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 提高許可權的 walkway。 |
| >>> waterRoute                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 渡船路徑線。 |
| >> water                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 任何像水的物體。 這包括海洋及溪流。 |
| >>> river                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 河流、溪流或其他水道。  請注意，這可能是線或多邊形，可能連接到非河流的水體。 |
| > routeMapElement                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 所有路由相關專案。 |
| >> routeLine                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 路由行相關專案。 |
| >>> drivingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表駕駛路線的線條。 |
| >>> scenicRoute                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表 scenic 駕駛路線的線條。 |
| >>> walkingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 代表步行路線的線條。 |
| > userMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 所有使用者專案。 |
| >> userBillboard                   | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 預設 [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 執行個體的樣式。 |
| >> userLine                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 預設 [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) 執行個體的樣式。 |
| >> userModel3D                     | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   |  ✔   | 預設 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 執行個體的樣式。  這主要是用於設定 renderAsSurface。 |
| >> userPoint                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 預設 [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) 執行個體的樣式。 |

<a id="properties" />

## <a name="properties"></a>屬性

本節說明可用於各個項目的屬性。

<a id="version" />

### <a name="version-properties"></a>版本屬性

| 屬性                     | 類型    | 描述                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| 版本                      | String  | 目標式樣式表版本。 用於適用性。 「1.0」為預設值「1.*」為其他次要功能更新。 |

<a id="settings" />

### <a name="settings-properties"></a>設定屬性

| 屬性                     | 類型    | 1703 | 1709 | 1803 | 1809 | 1903 | 描述 |
|------------------------------|---------|------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，表示氣象是否出現在 3D 控制項中。 |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   |  ✔   | 旗標，表示是否顯示具有紋理的符號 3D 建築的紋理。 |
| fogColor                     | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 出現在 3D 控制項的距離模糊 ARGB 色彩值。 |
| glowColor                    | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 可能會套用到標籤光暈與圖示光暈的 ARGB 色彩值。 |
| imageFamily                  | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用於此樣式的影像名稱 針對使用以真實世界標誌為依據之固定色彩的記號，將此值設為*Default*。 針對使用調色盤可調式色彩的記號，將這個值設為*Palette*。 |
| landColor                    | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 在土地上未繪製任何項目前，土地的 ARGB 色彩值。 |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，表示具有**Organization**屬性的項目是否應繪製適當的標誌或使用通用圖標。 |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，表示具有官方色彩屬性的項目（例如中國的中轉線）應繪製該色彩。 例如，關閉此值即為黑白地圖。 |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，指出是否繪製點陣區域，其中的表示方式比向量（日本和韓國）更好。 |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，指出是否在地圖上繪製海拔著色。 |
| shadowColor                  | 色彩   |      |      |      |  ✔   |  ✔   | 使用陰影之圖示後方的陰影色彩。 |
| spaceColor                   | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 地圖周圍區域的 ARGB 色彩值。 |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 旗標，指出是否應該使用 SVG 中的原始色彩，而不是查閱影像中色彩的調色板專案。 |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement 屬性

| 屬性                     | 類型    | 1703 | 1709 | 1803 | 1809 | 1903 | 描述 |
|------------------------------|---------|------|------|------|------|------|-------------|
| backgroundScale              | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 應該縮放的圖示的背景元素量。  例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| fillColor                    | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用來填滿多邊形、點圖示背景的色彩，如果線已分割，則用於填滿線的中央。 |
| fontFamily                   | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| fontWeight                   | String  |      |      |      |      |  ✔   | 字樣的密度（以筆劃的亮度或 heaviness 為依據）。 可以設定「**光線**」、「**標準**」、「**SemiBold**」和「**粗體**」 |
| iconColor                    | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 顯示於點圖示中間的字符色彩。 |
| iconScale                    | 浮點數   |      |  ✔   |  ✔   |  ✔   |  ✔   | 應該縮放的字符的背景元素量。  例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| labelColor                   | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 預設標籤大小的調整量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 讓 **FillColor** 的 alpha 值覆寫 **StrokeColor** 而不是與之混合。 |
| 小數點位數                        | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 整個點的大小調整量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| strokeColor                  | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 要用於多邊形邊框、點圖示邊框的色彩，以及線條色彩。 |
| strokeWidthScale             | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 線筆觸的調整量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

這個屬性群組繼承自[MapElement](#mapelement)屬性群組。

| 屬性                     | 類型    | 1703 | 1709 | 1803 | 1809 | 1903 | 描述 |
|------------------------------|---------|------|------|------|------|------|-------------|
| borderOutlineColor           | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 填滿的多邊形邊框的次要或外框線色彩。 |
| borderStrokeColor            | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 填滿的多邊形邊框的主要線色彩。 |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 框線筆劃的縮放量。 例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle 屬性

這個屬性群組繼承自[MapElement](#mapelement)屬性群組。

| 屬性                     | 類型    | 1703 | 1709 | 1803 | 1809 | 1903 | 描述 |
|------------------------------|---------|------|------|------|------|------|-------------|
| shadowVisible                | Bool    |      |      |      |      |  ✔   | 指出是否應該顯示圖示陰影的旗標 |
| 圖形-背景             | String  |      |      |      |      |  ✔   | 做為圖示背景使用的圖形--取代該處存在的任何圖形。 |
| 圖形-圖示                   | String  |      |      |      |      |  ✔   | 做為圖示前景圖像的圖形--取代該處存在的任何圖形。 |
| stemAnchorRadiusScale        | 浮點數   |      |      |  ✔   |  ✔   |  ✔   | 應該縮放的圖示主體的錨點量。  例如，使用 *1* 顯示預設和 *2* 以放大二倍。 |
| stemColor                    | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 在 3D 模式下，圖示底部出現的主體色彩。 |
| stemHeightScale              | 浮點數   |      |      |  ✔   |  ✔   |  ✔   | 應該縮放的圖示主體的長度量。  例如，使用 *1* 顯示預設和 *2* 以加長二倍。 |
| stemOutlineColor             | 色彩   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 在 3D 模式下，圖示底部出現的主體周圍邊框色彩。 |
| stemWidthScale               | 浮點數   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 應該縮放的圖示主體的寬度量。  例如，使用 *1* 顯示預設和 *2* 以加長二倍。 |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

這個屬性群組繼承自[MapElement](#mapelement)屬性群組。

| 屬性                     | 類型    | 1703 | 1709 | 1803 | 1809 | 1903 | 描述 |
|------------------------------|---------|------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   |  ✔   | 旗標，表示應轉譯的 3D 模型，例如建築，而無地面的深度淡出。 |
