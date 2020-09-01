---
title: 地圖和位置概觀
description: 本節說明如何在您的應用程式中顯示地圖、使用地圖服務、尋找位置，以及設定地理柵欄。 本節也示範如何啟動 Windows 地圖應用程式，以使用特定地圖、路線或一組轉向建議導航路線指引。
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 地圖, 位置, 地圖服務
ms.localizationpriority: medium
ms.openlocfilehash: c67312fe54492e20b6bb9a8b2d1cb07b5fc77c80
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171772"
---
# <a name="maps-and-location-overview"></a>地圖和位置概觀




本節說明如何在您的應用程式中顯示地圖、使用地圖服務、尋找位置，以及設定地理柵欄。 本節也示範如何啟動 Windows 地圖應用程式，以使用特定地圖、路線或一組轉向建議導航路線指引。

> [!TIP]
> 若要深入了解如何在應用程式中使用地圖和位置，請從 GitHub 上的 [Windows-universal-samples 存放庫](https://github.com/Microsoft/Windows-universal-samples)下載下列範例：
-   [通用 Windows 平台 (UWP) 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
-   [UWP 地理位置範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)

 

## <a name="display-maps"></a>顯示地圖


使用來自 [**Windows.UI.Xaml.Controls.Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps) 命名空間的 API，即可在您的應用程式中以 2D、3D 或 Streetside 檢視來顯示地圖。 您可以使用圖釘、影像、形狀或 XAML UI 元素，在地圖上標示興趣點 (POI)。 您也可以重疊顯示並排影像或完全取代地圖影像。

| 主題 | 描述 |
|-------|-------------|
| [要求地圖驗證金鑰](authentication-key.md) | 您的應用程式必須經過驗證，才能使用 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 和 [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) 命名空間中的地圖服務。 若要驗證您的應用程式，則必須指定地圖驗證金鑰。 本文說明如何從 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)要求地圖驗證金鑰，然後將金鑰新增到您的應用程式。 |
| [顯示地圖的 2D、3D 和 Streetside 檢視](display-maps.md) | 使用 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 類別，即可在您的應用程式中顯示可自訂的地圖。 本主題也會介紹空照圖 3D 和 Streetside 檢視。 |
| [在地圖上顯示興趣點 (POI)](display-poi.md) | 使用圖釘、影像、形狀及 XAML UI 元素，即可在地圖上新增興趣點 (POI)。 |
| [在地圖上重疊顯示並排影像](overlay-tiled-images.md) | 使用磚來源，即可在地圖上重疊顯示協力廠商或自訂的並排影像。 您可以使用磚來源來重疊顯示專業資訊，例如氣象資料、人口資料或地震資料，或是使用磚來源完全取代預設的地圖。 |



## <a name="access-map-services"></a>存取地圖服務

使用來自 [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) 命名空間的 API，即可將路線、路線指引及地理編碼功能新增到您的應用程式。

| 主題 | 描述 |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [要求地圖驗證金鑰](authentication-key.md) | 您的應用程式必須經過驗證，才能使用 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 和 [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) 命名空間中的地圖服務。 若要驗證您的應用程式，則必須指定地圖驗證金鑰。 本文說明如何從 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)要求地圖驗證金鑰，然後將金鑰新增到您的應用程式。 |
| [在地圖上顯示興趣點 (POI)](display-poi.md) | 使用圖釘、影像、形狀及 XAML UI 元素，即可在地圖上新增興趣點 (POI)。 |
| [顯示路線和路線指引](routes-and-directions.md) | 要求路線和路線指引，並在應用程式中加以顯示。 |
| [執行地理編碼和反向地理編碼](geocoding.md) | 呼叫 [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) 命名空間中 [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) 類別的方法，將地址轉換成地理位置 (地理編碼) 以及將地理位置轉換成地址 (反向地理編碼)。 |
| [尋找並下載地圖套件以供離線使用](/uwp/api/windows.services.maps.offlinemaps)| 在過去，您的應用程式必須將使用者導向 [設定] 應用程式才能下載離線地圖。 現在，您可以使用 [Windows.Services.Maps.OfflineMaps](/uwp/api/windows.services.maps.offlinemaps) 命名空間中的類別，在指定區域中尋找下載的套件 (根據 [Geopoint](/uwp/api/Windows.Devices.Geolocation.Geopoint)、[GeoboundingBox](/uwp/api/windows.devices.geolocation.geoboundingbox) 等)。 <br> 您也可以在使用者不需退出應用程式的情況下，檢查和接聽地圖套件的下載狀態，並開始下載。 <br> 您可找到如何在參考內容和[通用 Windows 平台 (UWP) 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)中執行此作業的範例。

## <a name="get-the-users-location"></a>取得使用者的位置

使用來自 [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) 命名空間的 API，即可讓您的應用程式取得使用者目前的位置，並在位置變更時收到通知。 這些 API 成員也常用在地圖 API 的參數中。 來自 [**Windows.Devices.Geolocation.Geofencing**](/uwp/api/Windows.Devices.Geolocation.Geofencing) 命名空間的 API，可讓您的應用程式在使用者進入或離開地理柵欄 (預先定義的地理區域) 時收到通知。

| 主題 | 描述 |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [要求地圖驗證金鑰](authentication-key.md) | 您的應用程式必須經過驗證，才能使用 [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 和 [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) 命名空間中的地圖服務。 若要驗證您的應用程式，則必須指定地圖驗證金鑰。 本文說明如何從 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)要求地圖驗證金鑰，然後將金鑰新增到您的應用程式。 |
| [定位感知應用程式的設計指導方針](guidelines-and-checklist-for-detecting-location.md) | 需要存取使用者位置的應用程式效能指導方針。 |
| [取得使用者的位置](get-location.md) | 取得使用者位置的存取權，並擷取位置。 | 
| [關於使用行止動線追蹤功能的指導方針](guidelines-for-visits.md) | 了解如何使用強大的「行止動線追蹤」(Visits Tracking) 功能，進行更切合實際的位置追蹤。 |
| [地理柵欄設計指導方針](guidelines-for-geofencing.md) | 使用地理柵欄功能之應用程式的效能指導方針。 |
| [設定地理柵欄](set-up-a-geofence.md) | 在您的應用程式中設定地理柵欄，並了解如何在前景和背景中處理通知。 |

## <a name="launch-the-windows-maps-app"></a>啟動 Windows 地圖應用程式

如這裡所示，您的應用程式可以啟動「Windows 地圖」應用程式以顯示特定的地圖和轉向建議導航路線指引。 請考慮使用「Windows 地圖」應用程式來提供地圖功能，而不要直接在您自己的應用程式中提供該功能。 如需詳細資訊，請參閱[啟動 Windows 地圖應用程式](../launch-resume/launch-maps-app.md)。

![Windows 地圖應用程式的範例。](images/mapnyc.png)

## <a name="related-topics"></a>相關主題

* [UWP 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [UWP 地理位置範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [取得目前的位置](get-location.md)
* [定位感知應用程式的設計指導方針](guidelines-and-checklist-for-detecting-location.md)
* [地圖的設計指導方針](./display-maps.md)
* [隱私權感知應用程式的設計指導方針](../security/index.md)
* [Build 2015 影片：跨手機、平板電腦和電腦運用 Windows app 中的地圖與位置功能](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 車流量應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)