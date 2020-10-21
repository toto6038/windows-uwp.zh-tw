---
title: 執行地理編碼和反向地理編碼
description: 本指南說明如何透過呼叫 Windows. Maps 命名空間中 MapLocationFinder 類別的方法，將街道位址轉換成地理位置 (地理編碼) 並將地理位置轉換成街道位址 (反向地理編碼) 。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, 地理編碼, 地圖, 位置
ms.localizationpriority: medium
ms.openlocfilehash: 992a9902081f0655885383ef90ea02ed1e79f13a
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297714"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>執行地理編碼和反向地理編碼

> [!NOTE]
> [**隨 mapcontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 和地圖服務會 Requite 稱為 [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken)的地圖服務驗證金鑰。 如需有關取得及設定地圖驗證金鑰的詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

本指南說明如何透過呼叫[**Windows. Maps**](/uwp/api/Windows.Services.Maps)命名空間中[**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)類別的方法，將街道位址轉換成地理位置 (地理編碼) 並將地理位置轉換成街道位址 (反向地理編碼) 。

> [!TIP]
> 若要深入瞭解如何在您的應用程式中使用地圖，請從 GitHub 上的[Windows 通用範例](hhttps://github.com/Microsoft/Windows-universal-samples)存放庫下載[隨 mapcontrol](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)範例。

地理編碼和反向地理編碼中包含的類別如下所示。

-   [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)類別包含的方法可處理地理編碼 ([**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) 和反向地理編碼 ([**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)) 。
-   這些方法都會傳回 [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 實例。
-   [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)的 [[**位置**](/uwp/api/windows.services.maps.maplocationfinderresult.locations)] 屬性會公開[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)物件的集合。 
-   [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) 物件都有 [**Address**](/uwp/api/windows.services.maps.maplocation.address) 屬性，它會公開代表街道位址的 [**MapAddress**](/uwp/api/Windows.Services.Maps.MapAddress) 物件，並使用 [**Point**](/uwp/api/windows.services.maps.maplocation.point) 屬性來公開代表地理位置的 [**Geopoint**](/uwp/api/windows.devices.geolocation.geopoint) 物件。

> [!IMPORTANT]
> 您必須指定地圖驗證金鑰，才能使用地圖服務。 如需詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

## <a name="get-a-location-geocode"></a>取得位置 (地理編碼)

本節說明如何將街道位址或地點名稱轉換成地理位置， (地理編碼) 。

1.  呼叫[**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)類別之[**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)方法的其中一個多載，其具有位置名稱或街道位址。
2.  [**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)方法會傳回[**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)物件。
3.  使用[**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)的 [[**位置**](/uwp/api/windows.services.maps.maplocationfinderresult.locations)] 屬性，即可公開集合[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)物件。 可能會有多個 [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) 物件，因為系統可能會找到多個對應至指定輸入的位置。

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

本節說明如何將地理位置轉換成位址 (反向地理編碼) 。

1.  呼叫 [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) 類別的 [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 方法。
2.  [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 方法會傳回 [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 物件，此物件包含相符之 [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) 物件的集合。
3.  使用[**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)的 [[**位置**](/uwp/api/windows.services.maps.maplocationfinderresult.locations)] 屬性，即可公開集合[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)物件。 可能會有多個 [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) 物件，因為系統可能會找到多個對應至指定輸入的位置。
4.  透過每個[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)的[**Address**](/uwp/api/windows.services.maps.maplocation.address)屬性存取[**MapAddress**](/uwp/api/Windows.Services.Maps.MapAddress)物件。

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
* [地圖的設計指導方針](./display-maps.md)
* [影片：在您的 Windows 應用程式中跨手機、平板電腦和 PC 運用地圖和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [**MapLocationFinder** 類別](/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync** 方法](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync** 方法](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
