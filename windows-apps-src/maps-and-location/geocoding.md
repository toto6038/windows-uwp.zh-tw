---
author: PatrickFarley
title: "執行地理編碼和反向地理編碼"
description: "呼叫 Windows.Services.Maps 命名空間中 MapLocationFinder 類別的方法，將地址轉換成地理位置 (地理編碼) 以及將地理位置轉換成地址 (反向地理編碼)。"
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: caf3ad6fecd6ed90c65f85477643fb42ab4787d3

---

# 執行地理編碼和反向地理編碼


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


呼叫 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空間中 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 類別的方法，將地址轉換成地理位置 (地理編碼) 以及將地理位置轉換成地址 (反向地理編碼)。

**提示**：若要深入了解如何在 app 中使用地圖，請從 GitHub 的 [Windows-universal-samples 存放庫](http://go.microsoft.com/fwlink/p/?LinkId=619979)下載下列範例。

-   [通用 Windows 平台 (UWP) 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)

以下說明地理編碼與反向地理編碼如何相關：

-   [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 類別擁有可執行地理編碼 ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) 和反向地理編碼 ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)) 的方法。
-   這些方法會傳回 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)。
-   [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 包含 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 物件的集合。 透過 **MapLocationFinderResult** 的 [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) 屬性存取這個集合。
-   每個 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 物件皆包含一個 [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) 物件。 請透過每個 **MapLocation** 的 [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) 屬性存取這個物件。

**重要** 您必須指定地圖驗證金鑰，才能使用地圖服務。 如需詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

 

## 取得位置 (地理編碼)


藉由執行下列步驟，即可將地址或地點名稱轉換成地理位置 (地理編碼)。

1.  呼叫 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 類別之 [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) 方法的其中一個多載。
2.  [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) 方法會傳回 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 物件，此物件包含相符之 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 物件的集合。
3.  透過 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 的 [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) 屬性存取這個集合。

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

這個程式碼會在 `tbOutputText` 文字方塊中顯示下列結果。

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## 取得地址 (反向地理編碼)


藉由執行下列步驟，即可將地理位置轉換成地址 (反向地理編碼)。

1.  呼叫 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 類別的 [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 方法。
2.  [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 方法會傳回 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 物件，此物件包含相符之 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 物件的集合。
3.  透過 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 的 [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) 屬性存取這個集合。
4.  透過每個 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 的 [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) 屬性存取 [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) 物件。

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

這個程式碼會在 `tbOutputText` 文字方塊中顯示下列結果。

``` syntax
town = Redmond
```

## 相關主題

* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [UWP 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [地圖的設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Build 2015 影片：跨手機、平板電腦和電腦運用 Windows app 中的地圖與位置功能](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 車流量 app 範例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)





<!--HONumber=Jun16_HO4-->


