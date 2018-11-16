---
author: normesta
title: 在地圖上顯示感興趣的地點 (POI)
description: 藉由使用圖釘、影像、圖形及 XAML UI 元素，即可在地圖上新增興趣點 (POI)。
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
ms.author: normesta
ms.date: 08/11/2017
ms.topic: article
keywords: Windows 10, UWP, 地圖, 位置, 圖釘
ms.localizationpriority: medium
ms.openlocfilehash: 13c0ea463cbab97e03c87c4e558bba0eff92300c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6980079"
---
# <a name="display-points-of-interest-on-a-map"></a>在地圖上顯示感興趣的地點

藉由使用圖釘、影像、圖形及 XAML UI 元素，即可在地圖上新增興趣點 (POI)。 POI 是地圖上代表感興趣項目的特定點。 例如，公司、城市或朋友的位置。

若要深入了解如何在應用程式中顯示 POI，請從 GitHub 的 [Windows-universal-samples 存放庫](http://go.microsoft.com/fwlink/p/?LinkId=619979) 下載下列範例：[通用 Windows 平台 (UWP) 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)。

藉由將 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)、 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)、[**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) 和 [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) 物件新增到 [**MapElementsLayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) 物件的 **MapElements** 集合，即可在地圖上顯示圖釘、影像及圖形。 然後，將該層物件新增到地圖控制項的 **層** 集錦中。

>[!NOTE]
> 在先前的版本中，本指南示範如何將地圖元素新增到 [**MapElements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) 集錦。 雖然您仍然可以使用此方式，但您會錯過新地圖層模型的一些優點。 若要深入了解，請參閱本指南的[使用層](#layers)章節。

藉由將 XAML 使用者介面元素新增到 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) 或 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的 [**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008)，您還可以在地圖上顯示這些元素，例如 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)、[**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 或 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)。

如果您大量的項目要放置在地圖上，請考慮[在地圖上重疊顯示並排影像](overlay-tiled-images.md)。 若要在地圖上顯示道路，請參閱[顯示路線和路線指引](routes-and-directions.md)

## <a name="add-a-pushpin"></a>新增圖釘

藉由使用 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 類別，即可在地圖上顯示含有選擇性文字的影像，例如圖釘。 您可以使用 [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 屬性來接受預設影像或提供自訂影像。 下圖顯示 **MapIcon** 的預設影像，其中 [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637088) 屬性分別為沒有指定值、有短標題、有長標題，以及很長的標題。

![範例 MapIcon 搭配各種不同長度的標題。](images/mapctrl-mapicons.png)

下列範例顯示西雅圖市的地圖，並新增含有預設影像與選擇性標題的 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)，以指出 Space Needle 的位置。 它還會將圖示放在地圖中央並放大。 如需使用地圖控制項的一般資訊，請參閱[顯示地圖的 2D、3D 和 Streetside 檢視](display-maps.md)。

```csharp
public void AddSpaceNeedleIcon()
{
    var MyLandmarks = new List<MapElement>();

    BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
    Geopoint snPoint = new Geopoint(snPosition);

    var spaceNeedleIcon = new MapIcon
    {
        Location = snPoint,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        ZIndex = 0,
        Title = "Space Needle"
    };

    MyLandmarks.Add(spaceNeedleIcon);

    var LandmarksLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarks
    };

    myMap.Layers.Add(LandmarksLayer);

    myMap.Center = snPoint;
    myMap.ZoomLevel = 14;

}
```

本範例說明地圖上的下列 POI (預設影像會在中央位置)。

![使用 MapIcon 的地圖](images/displaypoidefault.png)

下行程式碼會顯示含有自訂影像的 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)，而該影像儲存在專案的 Assets 資料夾中。 **MapIcon** 的 [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 屬性預期的值類型為 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813)。 這種類型會要求針對 [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) 命名空間**使用** using 陳述式。

>[!NOTE]
>如果您在多個地圖圖示都使用相同的影像，請在頁面或 App 層級宣告 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) 以獲得最佳效能。

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

使用 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 類別時，請將下列事項納入考量：

-   [**Image**](https://msdn.microsoft.com/library/windows/apps/dn637078) 屬性最大支援 2048x2048 像素的影像大小。
-   根據預設，不保證會顯示地圖圖示的影像。 當它遮蔽地圖上的其他元素或標籤時，可能會被隱藏。 若要將它保持在可見狀態，請將地圖圖示的 [**CollisionBehaviorDesired**](https://msdn.microsoft.com/library/windows/apps/dn974327) 屬性設定為 [**MapElementCollisionBehavior.RemainVisible**](https://msdn.microsoft.com/library/windows/apps/dn974314)。
-   並不保證會顯示 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637088) 的選擇性 [**Title**](https://msdn.microsoft.com/library/windows/apps/dn637077)。 如果您看不到文字，請降低 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637068) 的 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637004) 屬性值來縮小。
-   當您在地圖上顯示指向某特定位置的 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 影像 (例如圖釘或箭頭) 時，請考慮將 [**NormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637082) 屬性的值設定為影像上概略的指標位置。 如果您讓 **NormalizedAnchorPoint** 的值保留其預設值 (0, 0)，該值代表影像的左上角，變更地圖的 [**ZoomLevel**](https://msdn.microsoft.com/library/windows/apps/dn637068) 可能會讓影像指向不同的位置。
-   如果您沒有明確設定[高度](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.basicgeoposition)和 [AltitudeReferenceSystem](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint.AltitudeReferenceSystem)，則 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 將會放在表面上。

## <a name="add-a-3d-pushpin"></a>新增 3D 圖釘

您可以將三維物件新增到地圖。 使用 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 類別以從 [3D 製造格式 (3MF)](http://3mf.io/specification/) 檔案中匯入 3D 物件。

此影像使用 3D 咖啡杯標示附近咖啡店的位置。

![地圖上的馬克杯](images/mugs.png)

下列程式碼透過使用匯入 3MF 檔案新增咖啡杯到地圖。 為了簡單化，該程式碼將影像新增到地圖的中心，但是您的程式碼可能會將影像新增到特定位置。

```csharp
public async void Add3DMapModel()
{
    var mugStreamReference = RandomAccessStreamReference.CreateFromUri
        (new Uri("ms-appx:///Assets/mug.3mf"));

    var myModel = await MapModel3D.CreateFrom3MFAsync(mugStreamReference,
        MapModel3DShadingOption.Smooth);

    myMap.Layers.Add(new MapElementsLayer
    {
       ZIndex = 1,
       MapElements = new List<MapElement>
       {
          new MapElement3D
          {
              Location = myMap.Center,
              Model = myModel,
          },
       },
    });
}
```

## <a name="add-an-image"></a>新增影像

顯示與地圖位置相關的大型影像，如餐廳或地標的圖片。 當使用者縮小時，影像會等比例縮小，讓使用者可以檢視地圖的更多部分。 這與 [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077) 有點不同，後者標示特定位置，通常很小，且當使用者縮放地圖時仍維持相同的大小。

![MapBillboard 影像](images/map-billboard.png)

下列程式碼顯示上圖呈現的 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)。

```csharp
public void AddLandmarkPhoto()
{
    // Create MapBillboard.

    RandomAccessStreamReference mapBillboardStreamReference =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/billboard.jpg"));

    var mapBillboard = new MapBillboard(myMap.ActualCamera)
    {
        Location = myMap.Center,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        Image = mapBillboardStreamReference
    };

    // Add MapBillboard to a layer on the map control.

    var MyLandmarkPhotos = new List<MapElement>();

    MyLandmarkPhotos.Add(mapBillboard);

    var LandmarksPhotoLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarkPhotos
    };

    myMap.Layers.Add(LandmarksPhotoLayer);
}
```

此程式碼有三個部分值得進一步研究：影像、參考相機，以及 [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) 屬性。

### <a name="image"></a>影像

此範例顯示專案的 **\[資產\]** 資料夾中儲存的自訂影像。 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的 [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Image) 屬性預期的值類型為 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813)。 這種類型會要求針對 [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) 命名空間**使用** using 陳述式。

>[!NOTE]
>如果您在多個地圖圖示都使用相同的影像，請在頁面或應用程式層級宣告 [**RandomAccessStreamReference**](https://msdn.microsoft.com/library/windows/apps/hh701813) 以獲得最佳效能。

### <a name="reference-camera"></a>參考相機

 因為 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的影像隨著地圖的 [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) 變更而縮小放大，因此請務必定義 [**ZoomLevel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) 中影像顯示為正常比例 1 倍的位置。 此位置定義在 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的參考相機中，若要設定，您必須傳遞 [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 物件給 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的建構函式。

 您可以在 [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) 中定義您想要的位置，然後使用 [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) 建立 [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 物件。  不過，在此範例中，我們只使用地圖控制項的 [**ActualCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ActualCamera)屬性傳回的 [**MapCamera**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcamera) 物件。 這是地圖的內部相機。 該相機的目前位置將成為參考相機位置；亦即 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 影像顯示為 1 倍縮放比例的位置。

 如果您的應用程式可讓使用者在地圖上縮小，則因為地圖內部相機的高度上升而使得影像的大小降低，而當影像位於參考相機位置時則固定為 1 倍縮放比例。

### <a name="normalizedanchorpoint"></a>NormalizedAnchorPoint

[**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) 是錨定至 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的 [**Location**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) 屬性的影像點。 0.5,1 點是影像的底部中央。 因為我們已將 [**MapBillboard**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 的 [**Location**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) 屬性設定為地圖控制項的中央，所以影像的底部中央將錨定至地圖控制項的中央。 如果您希望影像直接位於某個點上，請將 [**NormalizedAnchorPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) 設定為 0.5,0.5。  

## <a name="add-a-shape"></a>加上圖形

藉由使用 [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103) 類別，即可在地圖上顯示多點圖形。 下列範例 (取自 [UWP 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)) 會在地圖上顯示帶有藍色邊框的紅色方塊。

```csharp
public void HighlightArea()
{
    // Create MapPolygon.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;

    var mapPolygon = new MapPolygon
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        ZIndex = 1,
        FillColor = Colors.Red,
        StrokeColor = Colors.Blue,
        StrokeThickness = 3,
        StrokeDashed = false,
    };

    // Add MapPolygon to a layer on the map control.
    var MyHighlights = new List<MapElement>();

    MyHighlights.Add(mapPolygon);

    var HighlightsLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyHighlights
    };

    myMap.Layers.Add(HighlightsLayer);
}
```

## <a name="add-a-line"></a>新增線條


藉由使用 [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114) 類別，即可在地圖上顯示線條。 下列範例 (取自 [UWP 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)) 會在地圖上顯示虛線。

```csharp
public void DrawLineOnMap()
{
    // Create Polyline.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;
    var mapPolyline = new MapPolyline
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        StrokeColor = Colors.Black,
        StrokeThickness = 3,
        StrokeDashed = true,
    };

   // Add Polyline to a layer on the map control.

   var MyLines = new List<MapElement>();

   MyLines.Add(mapPolyline);

   var LinesLayer = new MapElementsLayer
   {
       ZIndex = 1,
       MapElements = MyLines
   };

   myMap.Layers.Add(LinesLayer);

}
```

## <a name="add-xaml"></a>新增 XAML

藉由使用 XAML，即可在地圖上顯示自訂的 UI 元素。 藉由指定 XAML 的位置和正規化錨點，則可決定 XAML 在地圖上的位置。

-   呼叫 [**SetLocation**](https://msdn.microsoft.com/library/windows/desktop/ms704369) 以在地圖上設定放置 XAML 的位置。
-   呼叫 [**SetNormalizedAnchorPoint**](https://msdn.microsoft.com/library/windows/apps/dn637050) 以在 XAML 上設定與指定位置對應的相對位置。

下列範例顯示西雅圖市的地圖，並新增指出 Space Needle 位置的 XAML [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 控制項。 它還會將該區域放在地圖中央並放大。 如需使用地圖控制項的一般資訊，請參閱[顯示地圖的 2D、3D 和 Streetside 檢視](display-maps.md)。

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
   Geopoint snPoint = new Geopoint(snPosition);

   // Create a XAML border.
   Border border = new Border
   {
      Height = 100,
      Width = 100,
      BorderBrush = new SolidColorBrush(Windows.UI.Colors.Blue),
      BorderThickness = new Thickness(5),
   };

   // Center the map over the POI.
   MapControl1.Center = snPoint;
   MapControl1.ZoomLevel = 14;

   // Add XAML to the map.
   MapControl1.Children.Add(border);
   MapControl.SetLocation(border, snPoint);
   MapControl.SetNormalizedAnchorPoint(border, new Point(0.5, 0.5));
}
```

這個範例會在地圖上顯示藍色邊框。

![地圖之感興趣地點中所顯示 XAML 的螢幕擷取畫面](images/displaypoixaml.png)

接下來的範例示範如何使用資料繫結，直接在頁面的 XAML 標記中新增 XAML UI 元素。 和顯示內容的其他 XAML 元素一樣，[**Children**](https://msdn.microsoft.com/library/windows/apps/dn637008) 是 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的預設內容屬性，不需要在 XAML 標記中明確指定。

本範例說明如何將兩個 XAML 控制項顯示為 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 的隱含子系。 這些控制項出現在地圖上的資料繫結的位置。

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
</maps:MapControl>
```

透過使用程式碼後置檔案中的內容來設定這些位置。

```csharp
public Geopoint SeattleLocation { get; set; }
public Geopoint BellevueLocation { get; set; }
```

這個範例顯示如何顯示兩個中所包含的 XAML 控制項[**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094)。這些控制項出現在地圖上的資料繫結的位置。

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

本範例顯示繫結到 [**MapItemsControl**](https://msdn.microsoft.com/library/windows/apps/dn637094) 的 XAML 元素集合。

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{x:Bind LandmarkOverlays}">
      <maps:MapItemsControl.ItemTemplate>
          <DataTemplate>
              <StackPanel Background="Black" Tapped ="Overlay_Tapped">
                  <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Title}"
                    maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
              </StackPanel>
          </DataTemplate>
      </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

上述範例中的 ``ItemsSource`` 屬性繫結至程式碼後置檔案中的 [IList](https://docs.microsoft.com/dotnet/api/system.collections.ilist?view=netframework-4.70) 類型的内容。

```csharp
public sealed partial class Scenario1 : Page
{
    public IList LandmarkOverlays { get; set; }

    public MyClassConstructor()
    {
         SetLandMarkLocations();
         this.InitializeComponent();   
    }

    private void SetLandMarkLocations()
    {
        LandmarkOverlays = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        LandmarkOverlays.Add(pikePlaceIcon);

        var SeattleSpaceNeedleIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.6205, Longitude = -122.3493 }),
            Title = "Seattle Space Needle"
        };

        LandmarkOverlays.Add(SeattleSpaceNeedleIcon);
    }
}
```

<a id="layers" />

## <a name="working-with-layers"></a>使用層級

本指南中的範例將元素新增到 [MapElementLayers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) 集合中。 然後，顯示如何將該集合新增至地圖控制項的**層級**内容中。 在先前的版本中，本指南示範如何將地圖元素新增到 [**MapElements**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) 集錦，如下所示：

```csharp
var pikePlaceIcon = new MapIcon
{
    Location = new Geopoint(new BasicGeoposition
    { Latitude = 47.610, Longitude = -122.342 }),
    NormalizedAnchorPoint = new Point(0.5, 1.0),
    ZIndex = 0,
    Title = "Pike Place Market"
};

myMap.MapElements.Add(pikePlaceIcon);
```

雖然您仍然可以使用此方式，但您會錯過新地圖層模型的一些優點。 透過將元素分組到層級中，可以彼此獨立地操作每個層級。 例如，每一層都有自己的事件組，讓您可以回應特定層的特定事件，以及執行該事件專屬的動作。

此外，您可以將 XAML 直接繫結到 [MapLayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maplayer)。 這是您無法透過使用 [MapElements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) 集合來執行的某項目。

可以這麼做的一種方法是使用檢視模型類別、程式碼後置 XAML 頁面，和 XAML 頁面。

### <a name="view-model-class"></a>檢視模型類別

```csharp
public class LandmarksViewModel
{
    public ObservableCollection<MapLayer> LandmarkLayer
        { get; } = new ObservableCollection<MapLayer>();

    public LandmarksViewModel()
    {
        var MyElements = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        MyElements.Add(pikePlaceIcon);

        var LandmarksLayer = new MapElementsLayer
        {
            ZIndex = 1,
            MapElements = MyElements
        };

        LandmarkLayer.Add(LandmarksLayer);         
    }

```

### <a name="code-behind-a-xaml-page"></a>程式碼後置 XAML 頁面

將檢視模型類別連接到程式碼後置頁面。

```csharp
public LandmarksViewModel ViewModel { get; set; }

public myMapPage()
{
    this.InitializeComponent();
    this.ViewModel = new LandmarksViewModel();
}
```

### <a name="xaml-page"></a>XAML 頁面

在您的 XAML 頁面中，繫結至傳回該層級的檢視模型類別中的內容。

```XML
<maps:MapControl
    x:Name="myMap" TransitFeaturesVisible="False" Loaded="MyMap_Loaded" Grid.Row="2"
    MapServiceToken="Your token" Layers="{x:Bind ViewModel.LandmarkLayer}"/>
```

## <a name="related-topics"></a>相關主題

* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [UWP 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [地圖的設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Build 2015 影片：跨手機、平板電腦和電腦運用 Windows app 中的地圖與位置功能](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 車流量 app 範例](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapIcon**](https://msdn.microsoft.com/library/windows/apps/dn637077)
* [**MapPolygon**](https://msdn.microsoft.com/library/windows/apps/dn637103)
* [**MapPolyline**](https://msdn.microsoft.com/library/windows/apps/dn637114)
