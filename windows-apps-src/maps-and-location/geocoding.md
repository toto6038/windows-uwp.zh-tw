---
title: 執行地理編碼和反向地理編碼
description: 本指南說明如何將街道位址轉換成地理位置（地理編碼），並藉由呼叫 Windows. 地理編碼命名空間中 MapLocationFinder 類別的方法，將地理位置轉換成街道位址（反向）。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP, 地理編碼, 地圖, 位置
ms.localizationpriority: medium
ms.openlocfilehash: 5d7e1dda355cf87a2c8e26c11327cfff32e9d0b5
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259362"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>執行地理編碼和反向地理編碼

本指南說明如何將街道位址轉換成地理位置（地理編碼），並藉由呼叫[**Windows.** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)地理編碼命名空間中[**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)類別的方法，將地理位置轉換成街道位址（反向）。

> [!TIP]
> 若要深入瞭解如何在您的應用程式中使用對應，請從 GitHub 上的[Windows 通用範例](h https://github.com/Microsoft/Windows-universal-samples)存放庫下載[MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)範例。

與地理編碼和反向地理編碼相關的類別會以下列方式組織。

-   [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)類別包含處理地理編碼（[**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)）和反向地理編碼（[**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)）的方法。
-   這些方法都會傳回[**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)實例。
-   [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)的[**位置**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)屬性會公開[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件的集合。 
-   [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件同時具有[**Address**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address)屬性，它會公開代表街道位址的[**MapAddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress)物件，而[**Point**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point)屬性則會公開代表地理位置的[**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)物件。

> [!IMPORTANT]
> 您必須先指定地圖驗證金鑰，才能使用地圖服務。 如需詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

## <a name="get-a-location-geocode"></a>取得位置 (地理編碼)

本節說明如何將街道位址或地點名稱轉換成地理位置（地理編碼）。

1.  呼叫[**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)類別之[**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)方法的其中一個多載，並加上位置名稱或街道位址。
2.  [**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)方法會傳回[**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)物件。
3.  使用[**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)的 [[**位置**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)] 屬性來公開集合[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件。 可能會有多個[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件，因為系統可能會找到對應至指定輸入的多個位置。

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

本節說明如何將地理位置轉換成位址（反向地理編碼）。

1.  呼叫 [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 類別的 [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 方法。
2.  [  **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 方法會傳回 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 物件，此物件包含相符之 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 物件的集合。
3.  使用[**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)的 [[**位置**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)] 屬性來公開集合[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件。 可能會有多個[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件，因為系統可能會找到對應至指定輸入的多個位置。
4.  透過每個[**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)的[**Address**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address)屬性來存取[**MapAddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress)物件。

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

* [UWP 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [UWP 車流量應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [地圖的設計指導方針](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [影片：在 Windows 應用程式中跨電話、平板電腦和 PC 運用地圖和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [**MapLocationFinder**類別](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync**方法](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync**方法](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
