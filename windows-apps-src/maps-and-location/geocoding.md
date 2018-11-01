---
author: PatrickFarley
title: 執行地理編碼和反向地理編碼
description: 本指南會示範如何將街道地址轉換成地理位置 （地理編碼），以及將地理位置街道地址 （反向地理編碼） 來轉換成藉由呼叫 Windows.Services.Maps 命名空間中 MapLocationFinder 類別的方法。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.author: pafarley
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP, 地理編碼, 地圖, 位置
ms.localizationpriority: medium
ms.openlocfilehash: bdd956dece4435ceb8e14121ec2b545095af3a11
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5871935"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>執行地理編碼和反向地理編碼

本指南會示範如何將街道地址轉換成地理位置 （地理編碼），並藉由呼叫 Windows.Services.Maps[**中[**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)類別的方法轉換到街道地址 （反向地理編碼） 的地理位置**](https://msdn.microsoft.com/library/windows/apps/dn636979)命名空間。

> [!TIP]
> 若要深入了解您的應用程式中使用地圖，請從 GitHub 上的[Windows 通用範例存放庫](hhttps://github.com/Microsoft/Windows-universal-samples)下載[MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)範例。

地理編碼與反向地理編碼的相關類別的編排，如下所示。

-   [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)類別包含處理地理編碼 ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) 和反向地理編碼 ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)) 的方法。
-   這兩個這些方法會傳回[**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)執行個體。
-   [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)的[**位置**](https://msdn.microsoft.com/library/windows/apps/dn627552)屬性會公開[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)物件的集合。 
-   [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)物件有[**位址**](https://msdn.microsoft.com/library/windows/apps/dn636929)屬性，這會公開[**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533)物件，代表街道地址和[**點**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point)屬性，這會公開[**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)物件，代表地理位置。

> [!IMPORTANT]
> 您必須指定地圖驗證金鑰，才能使用地圖服務。 如需詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

## <a name="get-a-location-geocode"></a>取得位置 (地理編碼)

本節說明如何將街道地址或地點名稱轉換成地理位置 （地理編碼）。

1.  呼叫其中一個[**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)類別與街道地址或地點名稱的[**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)方法多載。
2.  [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)方法會傳回[**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)物件。
3.  您可以使用[**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)的[**位置**](https://msdn.microsoft.com/library/windows/apps/dn627552)屬性來公開集合[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)物件。 可能有多個[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)物件因為系統可能會發現對應到特定的輸入的多個位置。

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

## <a name="get-an-address-reverse-geocode"></a>取得地址 (反向地理編碼)

本節說明如何將地理位置轉換成地址 （反向地理編碼）。

1.  呼叫 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 類別的 [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 方法。
2.  [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) 方法會傳回 [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) 物件，此物件包含相符之 [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) 物件的集合。
3.  您可以使用[**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)的[**位置**](https://msdn.microsoft.com/library/windows/apps/dn627552)屬性來公開集合[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)物件。 可能有多個[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)物件因為系統可能會發現對應到特定的輸入的多個位置。
4.  透過每個[**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)的[**位址**](https://msdn.microsoft.com/library/windows/apps/dn636929)屬性存取[**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533)物件。

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

## <a name="related-topics"></a>相關主題

* [UWP 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP 車流量 app 範例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [地圖的設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [影片： 跨手機、 平板電腦與電腦在 Windows 應用程式中的運用地圖和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [**MapLocationFinder** 類別](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync**方法](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync**方法](https://msdn.microsoft.com/library/windows/apps/dn636928)
