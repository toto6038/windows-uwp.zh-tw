---
title: 執行地理編碼和反向地理編碼
description: 本指南將說明如何將街道地址轉換成地理位置 （地理編碼），並藉由呼叫類別的方法 MapLocationFinder Windows.Services.Maps 命名空間中轉換到街道地址 （反向地理編碼） 的地理位置。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP, 地理編碼, 地圖, 位置
ms.localizationpriority: medium
ms.openlocfilehash: 88687f6e7074ff7c914927a81a08720336b3c60c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371877"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>執行地理編碼和反向地理編碼

本指南說明如何將街道地址轉換為地理位置 （地理編碼），並藉由呼叫的方法轉換成街道地址 （反向地理編碼） 地理位置[ **MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)類別內[ **Windows.Services.Maps** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)命名空間。

> [!TIP]
> 若要深入了解應用程式中使用對應，下載[MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)範例[Windows 通用範例存放庫](h https://github.com/Microsoft/Windows-universal-samples)GitHub 上。

組織參與地理編碼和反向地理編碼的類別，如下所示。

-   [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)類別包含處理地理編碼的方法 ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) 和反向地理編碼 ([**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync))。
-   這兩個這些方法會傳回[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)執行個體。
-   [**位置**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)屬性[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)公開的一組[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件。 
-   [**MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件具有同時[**位址**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address)屬性，它會公開[ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress)物件，代表街道地址，以及[**點**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point)屬性，它會公開[ **Geopoint** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint)物件代表地理位置。

> [!IMPORTANT]
> 您可以使用對應服務之前，您必須指定對應的驗證金鑰。 如需詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

## <a name="get-a-location-geocode"></a>取得位置 (地理編碼)

本節說明如何將街道地址或位置名稱轉換成地理位置 （地理編碼）。

1.  呼叫其中一個多載[ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)方法[ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)類別的名稱或街道地址。
2.  [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)方法會傳回[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)物件。
3.  使用[**位置**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)屬性[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)公開集合[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件。 可能有多個[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件，因為系統可能會發現多個對應至給定的輸入的位置。

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

本節說明如何將轉換的地理位置的位址 （反向地理編碼）。

1.  呼叫 [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) 類別的 [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 方法。
2.  [  **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 方法會傳回 [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 物件，此物件包含相符之 [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) 物件的集合。
3.  使用[**位置**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations)屬性[ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)公開集合[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件。 可能有多個[ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)物件，因為系統可能會發現多個對應至給定的輸入的位置。
4.  存取權[ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress)物件透過[**位址**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address)屬性，每個[ **MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation).

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

* [UWP 的對應範例](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP 流量的應用程式範例](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [對應的設計方針](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [影片：利用跨電話、 平板電腦和 PC 在 Windows 應用程式中的地圖與位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [**MapLocationFinder** class](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync**方法](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync**方法](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
