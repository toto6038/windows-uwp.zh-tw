---
author: msatranjr
title: "顯示 2D、3D 和 Streetside 檢視的地圖"
description: "藉由使用 MapControl 類別，即可在您的 app 中顯示可自訂的地圖。 本主題也會介紹空照圖 3D 和 Streetside 檢視。"
ms.assetid: 3839E00B-2C1E-4627-A45F-6DDA98D7077F
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: 249503f6a43ef8c38e76ed29aed4a1bfdb26e9fb

---

# 顯示 2D、3D 和 Streetside 檢視的地圖


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


藉由使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 類別，即可在您的 app 中顯示可自訂的地圖。 本主題也會介紹空照圖 3D 和 Streetside 檢視。

**提示**：若要深入了解如何在 app 中使用地圖，請從 GitHub 的 [Windows-universal-samples 存放庫](http://go.microsoft.com/fwlink/p/?LinkId=619979)下載下列範例。

-   [通用 Windows 平台 (UWP) 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## 將地圖控制項新增至您的 app


藉由新增 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)，在 XAML 頁面上顯示地圖。 若要使用 **MapControl**，您必須在 XAML 頁面或您的程式碼中宣告 [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) 命名空間。 如果您從工具箱拖曳控制項，會自動新增此命名空間宣告。 如果您是手動將 **MapControl** 新增到 XAML 頁面，就必須在頁面頂端手動新增命名空間宣告。

下列範例顯示基本的地圖控制項，並設定讓地圖在除了接受觸控輸入之外，還能顯示縮放和傾斜控制項。 如需有關自訂地圖外觀的詳細資訊，請參閱[設定地圖](#mapconfig)。

```xml
<Page
    x:Class="MapsAndLocation1.DisplayMaps"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MapsAndLocation1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:Maps="using:Windows.UI.Xaml.Controls.Maps"
    mc:Ignorable="d">

 <Grid x:Name="pageGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Maps:MapControl
       x:Name="MapControl1"            
       ZoomInteractionMode="GestureAndControl"
       TiltInteractionMode="GestureAndControl"   
       MapServiceToken="EnterYourAuthenticationKeyHere"/>
  
 </Grid>
</Page>
```

如果您是在程式碼中新增地圖控制項，就必須在程式碼檔案頂端手動宣告命名空間。

```csharp
using Windows.UI.Xaml.Controls.Maps;
...

// Add the MapControl and the specify maps authentication key.
MapControl MapControl2 = new MapControl();
MapControl2.ZoomInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.TiltInteractionMode = MapInteractionMode.GestureAndControl;
MapControl2.MapServiceToken = "EnterYourAuthenticationKeyHere";
pageGrid.Children.Add(MapControl2);
```

## 取得及設定地圖驗證金鑰


您必須先指定地圖驗證金鑰做為 [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036) 屬性的值，才能使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和地圖服務。 請在先前的範例中，使用您從 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)取得的金鑰來取代 `EnterYourAuthenticationKeyHere`。 除非您指定地圖驗證金鑰，否則「**警告: 未指定 MapServiceToken**」文字會持續顯示在的控制項下方。 如需有關取得及設定地圖驗證金鑰的詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

## 設定地圖的開始位置


藉由在您的程式碼中指定 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) 屬性，或在您的 XAML 標記中繫結該屬性，即可設定要在地圖上顯示的位置。 下列範例會顯示以西雅圖市為中心的地圖。

**提示**：由於字串無法轉換成 [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675)，因此您無法在 XAML 標記中指定 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) 屬性的值，除非您使用資料繫結。 (這項限制也適用於 [**MapControl.Location**](https://msdn.microsoft.com/library/windows/apps/dn653264) 附加屬性。)

 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Specify a known location.
   BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };
   Geopoint cityCenter = new Geopoint(cityPosition);

   // Set the map location.
   MapControl1.Center = cityCenter;
   MapControl1.ZoomLevel = 12;
   MapControl1.LandmarksVisible = true;
}
```

![地圖控制項的範例。](images/displaymapsexample1.png)

## 設定地圖的目前位置


您的 app 必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) 方法，才能存取使用者的位置。 此時，您的 app 必須在前景，且 **RequestAccessAsync** 必須是從 UI 執行緒呼叫。 在使用者授與您的 app 存取其位置的權限之前，您的 app 將無法存取位置資料。

藉由使用 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 類別的 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 方法，即可取得裝置的目前位置 (如果位置可以使用)。 若要取得對應的 [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675)，您可以使用地理位置坐標的 [**Point**](https://msdn.microsoft.com/library/windows/apps/dn263665) 屬性。 如需詳細資訊，請參閱[取得目前位置](get-location.md)。

```csharp
// Set your current location.
var accessStatus = await Geolocator.RequestAccessAsync();
switch (accessStatus)
{
   case GeolocationAccessStatus.Allowed:

      // Get the current location.
      Geolocator geolocator = new Geolocator();
      Geoposition pos = await geolocator.GetGeopositionAsync();
      Geopoint myLocation = pos.Coordinate.Point;

      // Set the map location.
      MapControl1.Center = myLocation;
      MapControl1.ZoomLevel = 12;
      MapControl1.LandmarksVisible = true;
      break;

   case GeolocationAccessStatus.Denied:
      // Handle the case  if access to location is denied.
      break;

   case GeolocationAccessStatus.Unspecified:
      // Handle the case if  an unspecified error occurs.
      break;
}
```

在地圖上顯示您的裝置位置時，請考慮顯示圖形，並根據位置資料的正確性設定縮放比例。 如需詳細資訊，請參閱[定位感知應用程式的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465148)。

## 變更地圖的位置。


若要變更顯示於 2D 地圖的位置，您可以呼叫 [**TrySetViewAsync**](https://msdn.microsoft.com/library/windows/apps/dn637060) 方法的其中一個多載。 使用該方法來為 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005)、[**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068)、[**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) 和 [**Pitch**](https://msdn.microsoft.com/library/windows/apps/dn637044) 指定新的值。 您也可以提供 [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) 列舉中的常數，以指定要在檢視變更時使用的選用動畫。

若要變更 3D 地圖的位置，您可以改用 [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296) 方法。 如需詳細資訊，請參閱[顯示 3D 檢視](#display3d)。

呼叫 [**TrySetViewBoundsAsync**](https://msdn.microsoft.com/library/windows/apps/dn637065) 方法，在地圖上顯示 [**GeoboundingBox**](https://msdn.microsoft.com/library/windows/apps/dn607949) 的內容。 例如，您可以使用此方法在地圖上顯示一條路線或路線的一部分。 如需詳細資訊，請參閱[在地圖上顯示路線和路線指引](routes-and-directions.md)。

## 設定地圖


設定下列 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 屬性的值，以設定地圖與其外觀。

**地圖設定**

-   設定 [**Center**](https://msdn.microsoft.com/library/windows/apps/dn637005) 屬性，將地圖的**中心**設定為某個地理點。
-   將 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) 屬性設定為 1 到 20 之間的值，以設定地圖的**縮放層級**。
-   設定 [**Heading**](https://msdn.microsoft.com/library/windows/apps/dn637019) 屬性以設定地圖的**旋轉**，其中 0 或 360 度 = 北、90 = 東、180 = 南、270 = 西。
-   將 [**DesiredPitch**](https://msdn.microsoft.com/library/windows/apps/dn637012) 屬性設定為 0 到 65 度之間的值，以設定地圖的**傾斜**。

**地圖外觀**

-   利用其中一個 [**MapStyle**](https://msdn.microsoft.com/library/windows/apps/dn637127) 常數設定 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) 屬性，以指定地圖的**類型** (例如，路面地圖或空照圖地圖)。
-   利用其中一個 [**MapColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637003) 常數設定 [**ColorScheme**](https://msdn.microsoft.com/library/windows/apps/dn637010) 屬性，將地圖的**色彩配置**設定為淺色或深色。

設定下列 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 屬性的值，顯示地圖上的資訊。

-   啟用或停用 [**LandmarksVisible**](https://msdn.microsoft.com/library/windows/apps/dn637023) 屬性，在地圖上顯示**建築物與地標**。
-   啟用或停用 [**PedestrianFeaturesVisible**](https://msdn.microsoft.com/library/windows/apps/dn637042) 屬性，在地圖上顯示**行人功能**，例如公共樓梯。
-   啟用或停用 [**TrafficFlowVisible**](https://msdn.microsoft.com/library/windows/apps/dn637055) 屬性，在地圖上顯示**交通**。
-   將 [**WatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn637066) 屬性設定成其中一個 [**MapWatermarkMode**](https://msdn.microsoft.com/library/windows/apps/dn610749) 常數，以指定是否要在地圖上顯示**浮水印**。
-   將 [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122) 新增到地圖控制項的 [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) 集合，以在地圖上顯示**開車或步行路線**。 如需詳細資訊和範例，請參閱[在地圖上顯示路線和路線指引](routes-and-directions.md)。

如需有關如何在 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 中顯示圖釘、圖形及 XAML 控制項的資訊，請參閱[在地圖上顯示興趣點 (POI)](display-poi.md)。

## 顯示 Streetside 檢視


Streetside 檢視是位置的街景透視，它會出現在地圖控制項上方。

![地圖控制項的 Streetside 檢視範例。](images/onlystreetside-730width.png)

考慮「走進」Streetside 檢視的經驗，這不同於原先在地圖控制項中顯示的地圖。 例如，變更 Streetside 檢視中的位置並不會變更 Streetside 檢視「底下」的地圖位置或外觀。 關閉 Streetside 檢視之後 (方法是按一下控制項右上角的 **X**)，原始的地圖會保持不變。

顯示 Streetside 檢視：

1.  檢查 [**IsStreetsideSupported**](https://msdn.microsoft.com/library/windows/apps/dn974271) 以判斷裝置是否支援 Streetside 檢視。
2.  如果支援 Streetside 檢視，請呼叫 [**FindNearbyAsync**](https://msdn.microsoft.com/library/windows/apps/dn974361) 以在指定位置附近建立 [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360)。
3.  檢查 [**StreetsidePanorama**](https://msdn.microsoft.com/library/windows/apps/dn974360) 是否不是 Null，以判斷是否有找到附近的全景。
4.  如果有找到附近的全景，請為地圖控制項的 [**CustomExperience**](https://msdn.microsoft.com/library/windows/apps/dn974263) 屬性建立 [**StreetsideExperience**](https://msdn.microsoft.com/library/windows/apps/dn974356)。

本範例說明如何顯示類似先前影像的 Streetside 檢視。

**注意**：如果地圖控制項的比例太小，則不會出現概觀地圖。

 

```csharp
private async void showStreetsideView()
{
   // Check if Streetside is supported.
   if (MapControl1.IsStreetsideSupported)
   {
      // Find a panorama near Avenue Gustave Eiffel.
      BasicGeoposition cityPosition = new BasicGeoposition() { Latitude = 48.858, Longitude = 2.295};
      Geopoint cityCenter = new Geopoint(cityPosition);
      StreetsidePanorama panoramaNearCity = await StreetsidePanorama.FindNearbyAsync(cityCenter);

      // Set the Streetside view if a panorama exists.
      if (panoramaNearCity != null)
      {
         // Create the Streetside view.
         StreetsideExperience ssView = new StreetsideExperience(panoramaNearCity);
         ssView.OverviewMapVisible = true;
         MapControl1.CustomExperience = ssView;
      }
   }
   else
   {
      // If Streetside is not supported
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "Streetside is not supported",
         Content ="\nStreetside views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();            
   }
}
```

## 顯示空照圖 3D 檢視


藉由使用 [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329) 類別，即可指定地圖的 3D 透視。 地圖場景代表在地圖中顯示的 3D 檢視。 [
            **MapCamera**](https://msdn.microsoft.com/library/windows/apps/dn974244) 類別代表顯示這類檢視的相機位置。

![](images/mapcontrol-techdiagram.png)

若要讓地圖表面上的建築物和其他功能以 3D 方式顯現，請將地圖控制項的 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) 屬性設定為 [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127)。 以下是使用 **Aerial3DWithRoads** 樣式的 3D 檢視範例。

![3D 地圖檢視的範例。](images/only3d-730width.png)

顯示 3D 檢視

1.  檢查 [**Is3DSupported**](https://msdn.microsoft.com/library/windows/apps/dn974265) 以判斷裝置是否支援 3D 檢視。
2.  如果支援 3D 檢視，請將地圖控制項的 [**Style**](https://msdn.microsoft.com/library/windows/apps/dn637051) 屬性設定為 [**MapStyle.Aerial3DWithRoads**](https://msdn.microsoft.com/library/windows/apps/dn637127)。
3.  使用眾多 **CreateFrom** 方法的其中之一 (例如 [**CreateFromLocationAndRadius**](https://msdn.microsoft.com/library/windows/apps/dn974336) 和 [**CreateFromCamera**](https://msdn.microsoft.com/library/windows/apps/dn974334)) 來建立 [**MapScene**](https://msdn.microsoft.com/library/windows/apps/dn974329) 物件。
4.  呼叫 [**TrySetSceneAsync**](https://msdn.microsoft.com/library/windows/apps/dn974296) 以顯示 3D 檢視。 您也可以提供 [**MapAnimationKind**](https://msdn.microsoft.com/library/windows/apps/dn637002) 列舉中的常數，以指定要在檢視變更時使用的選用動畫。

本範例說明如何顯示 3D 檢視。

```csharp
private async void display3DLocation()
{
   if (MapControl1.Is3DSupported)
   {
      // Set the aerial 3D view.
      MapControl1.Style = MapStyle.Aerial3DWithRoads;

      // Specify the location.
      BasicGeoposition hwGeoposition = new BasicGeoposition() { Latitude = 34.134, Longitude = -118.3216};
      Geopoint hwPoint = new Geopoint(hwGeoposition);

      // Create the map scene.
      MapScene hwScene = MapScene.CreateFromLocationAndRadius(hwPoint,
                                                                           80, /* show this many meters around */
                                                                           0, /* looking at it to the North*/
                                                                           60 /* degrees pitch */);
      // Set the 3D view with animation.
      await MapControl1.TrySetSceneAsync(hwScene,MapAnimationKind.Bow);
   }
   else
   {
      // If 3D views are not supported, display dialog.
      ContentDialog viewNotSupportedDialog = new ContentDialog()
      {
         Title = "3D is not supported",
         Content = "\n3D views are not supported on this device.",
         PrimaryButtonText = "OK"
      };
      await viewNotSupportedDialog.ShowAsync();
   }
}
```

## 取得位置與元素的相關資訊


藉由呼叫下列 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 方法，即可取得地圖上位置的相關資訊。

-   [
            **GetLocationFromOffset**](https://msdn.microsoft.com/library/windows/apps/dn637016) 方法 - 取得與地圖控制項檢視區中指定的點對應的地理位置。
-   [
            **GetOffsetFromLocation**](https://msdn.microsoft.com/library/windows/apps/dn637018) 方法 - 取得與指定的地理位置對應的地圖控制項檢視區中的點。
-   [
            **IsLocationInView**](https://msdn.microsoft.com/library/windows/apps/dn637022) 方法 - 決定目前是否能在地圖控制項的檢視區中看見指定的地理位置。
-   [
            **FindMapElementsAtOffset**](https://msdn.microsoft.com/library/windows/apps/dn637014) 方法 - 取得地圖上位於地圖控制項檢視區中指定點的元素。

## 處理使用者互動及變更


藉由處理下列 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 事件，即可處理使用者在地圖上的輸入手勢。 請檢查 [**MapInputEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn637090) 之 [**Location**](https://msdn.microsoft.com/library/windows/apps/dn637091) 和 [**Position**](https://msdn.microsoft.com/library/windows/apps/dn637093) 屬性的值，以取得手勢發生所在的地圖上地理位置及檢視區中實體位置的相關資訊。

-   [**MapTapped**](https://msdn.microsoft.com/library/windows/apps/dn637038)
-   [**MapDoubleTapped**](https://msdn.microsoft.com/library/windows/apps/dn637032)
-   [**MapHolding**](https://msdn.microsoft.com/library/windows/apps/dn637035)

藉由處理控制項的 [**LoadingStatusChanged**](https://msdn.microsoft.com/library/windows/apps/dn637028) 事件，即可判斷地圖是正在載入，還是已完全載入。

請處理下列 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 事件，以處理使用者或應用程式變更地圖設定時所發生的變更。 [地圖的指導方針](https://msdn.microsoft.com/library/windows/apps/dn596102)

-   [**CenterChanged**](https://msdn.microsoft.com/library/windows/apps/dn637006)
-   [**HeadingChanged**](https://msdn.microsoft.com/library/windows/apps/dn637020)
-   [**PitchChanged**](https://msdn.microsoft.com/library/windows/apps/dn637045)
-   [**ZoomLevelChanged**](https://msdn.microsoft.com/library/windows/apps/dn637069)

## 相關主題

* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [UWP 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [取得目前的位置](get-location.md)
* [定位感知 app 的設計指導方針](https://msdn.microsoft.com/library/windows/apps/hh465148)
* [地圖的設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Build 2015 影片：跨手機、平板電腦和電腦運用 Windows app 中的地圖與位置功能](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 車流量 app 範例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)





<!--HONumber=Jun16_HO4-->


