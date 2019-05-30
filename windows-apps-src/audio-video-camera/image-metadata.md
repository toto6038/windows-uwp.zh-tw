---
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: 本文示範如何讀取和寫入影像中繼資料屬性，以及如何使用 GeotagHelper 公用程式類別以標記檔案的地理位置。
title: 影像中繼資料
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 589dd40282fc3186f225e3873295863cc41b6a0c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361711"
---
# <a name="image-metadata"></a>影像中繼資料



本文示範如何讀取和寫入影像中繼資料屬性，以及如何使用 [**GeotagHelper**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.GeotagHelper) 公用程式類別以標記檔案的地理位置。

## <a name="image-properties"></a>映像內容

[  **StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties) 屬性會傳回 [**StorageItemContentProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemContentProperties) 物件，這個物件可以用來存取檔案的內容相關資訊。 呼叫 [**GetImagePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.getimagepropertiesasync) 取得影像特定的屬性。 傳回的 [**ImageProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.ImageProperties) 物件所公開的成員包含基本影像中繼資料欄位，例如影像標題和拍攝日期。

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

若要存取更大的一組檔案中繼資料，請使用 Windows 屬性系統，這是一組可使用唯一字串識別碼擷取的檔案中繼資料屬性。 針對您想要擷取的每個屬性，建立字串清單並加入識別碼。 [  **ImageProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.imageproperties.retrievepropertiesasync) 方法接受此字串清單，並傳回機碼/值組字典，其中機碼是屬性識別碼，值是屬性值。

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   如需完整的 Windows 屬性清單，包括每個屬性的識別碼和類型，請參閱 [Windows 屬性](https://docs.microsoft.com/windows/desktop/properties/props)。

-   某些檔案容器和影像轉碼器只支援部分屬性。 如需每個影像類型支援的影像中繼資料清單，請參閱[相片中繼資料原則](https://docs.microsoft.com/windows/desktop/wic/photo-metadata-policies)。

-   因為擷取不支援的屬性時可能傳回 Null 值，在使用傳回的中繼資料值之前，一定要先檢查是否為 Null。

## <a name="geotag-helper"></a>地理位置標籤協助程式

GeotagHelper 是公用程式類別，可讓您直接使用 [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) API，輕鬆地以地理位置資料來標記影像，而不必手動剖析或建構中繼資料格式。

如果您已經有 [**Geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) 物件代表您想要標記在影像中的位置 (可能從先前使用的地理位置 API 或其他來源)，您可以呼叫 [**GeotagHelper.SetGeotagAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagasync) 並傳入 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 和 **Geopoint**，以設定地理位置標籤資料。

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

若要使用裝置的目前位置來設定地理位置標籤資料，請建立新的 [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) 物件並呼叫 [**GeotagHelper.SetGeotagFromGeolocatorAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync)，同時傳入 **Geolocator** 和要標記的檔案。

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   您必須在 app 資訊清單中包含 **location** 裝置功能，才能使用 [**SetGeotagFromGeolocatorAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) API。

-   您必須先呼叫 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync)，再呼叫 [**SetGeotagFromGeolocatorAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync)，以確保使用者已授權您的 app 來使用他們的位置。

-   如需地理位置 API 的詳細資訊，請參閱[地圖與位置](https://docs.microsoft.com/windows/uwp/maps-and-location/index)。

若要取得代表影像檔地理位置的 GeoPoint，請呼叫 [**GetGeotagAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.geotaghelper.getgeotagasync)。

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## <a name="decode-and-encode-image-metadata"></a>解碼和編碼影像中繼資料

最先進的影像資料處理方式是使用 [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 或 [BitmapEncoder](bitmapencoder-options-reference.md)，在資料流層級上讀取及寫入屬性。 對於這些作業，您可以使用 Windows 屬性來指定您要讀取或寫入的資料，但您也可以使用 Windows 影像處理元件 (WIC) 提供的中繼資料查詢語言，指定所要求屬性的路徑。

使用這項技術來讀取影像中繼資料時，您需要有以來源影像檔資料流所建立的 [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder)。 相關作法請參閱[影像處理](imaging.md)。

當您有解碼器時，針對您想要擷取的每個中繼資料屬性，請使用 Windows 屬性識別碼字串或 WIC 中繼資料查詢，建立字串清單並加入新項目。 在解碼器的 [**BitmapProperties**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapProperties) 成員上呼叫 [**BitmapPropertiesView.GetPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) 方法，以要求指定的屬性。 屬性會以機碼/值組的字典傳回，其中包含屬性名稱或路徑及屬性值。

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   如需 WIC 中繼資料查詢語言和支援的屬性的相關資訊，請參閱 [WIC 影像格式原生中繼資料查詢](https://docs.microsoft.com/windows/desktop/wic/-wic-native-image-format-metadata-queries)。

-   許多中繼資料屬性只有在一部分影像類型中才支援。 [**GetPropertiesAsync** ](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync)如果其中一個要求的屬性不支援映像相關聯的解碼器和 0x88982F81 若映像不完全支援中繼資料將會失敗並告知錯誤碼 0x88982F41。 這些錯誤碼與相關聯的常數是 WINCODEC\_ERR\_PROPERTYNOTSUPPORTED 和 WINCODEC\_ERR\_UNSUPPORTEDOPERATION 並且定義於 winerror.h 標頭檔。
-   因為影像可能包含或不包含特定屬性的值，在嘗試存取屬性之前，請使用 **IDictionary.ContainsKey** 確認屬性存在於結果中。

將影像中繼資料寫入資料流時，需要有影像輸出檔相關聯的 **BitmapEncoder**。

建立 [**BitmapPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) 物件以包含您要設定的屬性值。 建立 [**BitmapTypedValue**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) 物件以代表屬性值。 這個物件使用 **object** 做為值，並使用 [**PropertyType**](https://docs.microsoft.com/uwp/api/Windows.Foundation.PropertyType) 列舉的成員來定義值的類型。 將 **BitmapTypedValue** 加入到 **BitmapPropertySet**，然後呼叫 [**BitmapProperties.SetPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync)，使編碼器將屬性寫入資料流。

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   如需哪些影像檔類型支援哪些屬性的詳細資訊，請參閱 [Windows 屬性](https://docs.microsoft.com/windows/desktop/properties/props)、[相片中繼資料原則](https://docs.microsoft.com/windows/desktop/wic/photo-metadata-policies)和 [WIC 影像格式原生中繼資料查詢](https://docs.microsoft.com/windows/desktop/wic/-wic-native-image-format-metadata-queries)。

-   [**SetPropertiesAsync** ](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync)編碼器相關聯的映像不支援要求的屬性之一，將會失敗並錯誤碼 0x88982F41。

## <a name="related-topics"></a>相關主題

* [映像](imaging.md)
 

 




