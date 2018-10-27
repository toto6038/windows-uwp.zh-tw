---
author: laurenhughes
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: 本文示範如何讀取和寫入影像中繼資料屬性，以及如何使用 GeotagHelper 公用程式類別以標記檔案的地理位置。
title: 影像中繼資料
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a3e2f10174412b49ce60f3da6a4bb73b2efc4411
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5695932"
---
# <a name="image-metadata"></a>影像中繼資料



本文示範如何讀取和寫入影像中繼資料屬性，以及如何使用 [**GeotagHelper**](https://msdn.microsoft.com/library/windows/apps/dn903683) 公用程式類別以標記檔案的地理位置。

## <a name="image-properties"></a>影像屬性

[**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225) 屬性會傳回 [**StorageItemContentProperties**](https://msdn.microsoft.com/library/windows/apps/hh770642) 物件，這個物件可以用來存取檔案的內容相關資訊。 呼叫 [**GetImagePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770646) 取得影像特定的屬性。 傳回的 [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/br207718) 物件所公開的成員包含基本影像中繼資料欄位，例如影像標題和拍攝日期。

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

若要存取更大的一組檔案中繼資料，請使用 Windows 屬性系統，這是一組可使用唯一字串識別碼擷取的檔案中繼資料屬性。 針對您想要擷取的每個屬性，建立字串清單並加入識別碼。 [**ImageProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br207732) 方法接受此字串清單，並傳回機碼/值組字典，其中機碼是屬性識別碼，值是屬性值。

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   如需完整的 Windows 屬性清單，包括每個屬性的識別碼和類型，請參閱 [Windows 屬性](https://msdn.microsoft.com/library/windows/desktop/dd561977)。

-   某些檔案容器和影像轉碼器只支援部分屬性。 如需每個影像類型支援的影像中繼資料清單，請參閱[相片中繼資料原則](https://msdn.microsoft.com/library/windows/desktop/ee872003)。

-   因為擷取不支援的屬性時可能傳回 Null 值，在使用傳回的中繼資料值之前，一定要先檢查是否為 Null。

## <a name="geotag-helper"></a>地理位置標籤協助程式

GeotagHelper 是公用程式類別，可讓您直接使用 [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) API，輕鬆地以地理位置資料來標記影像，而不必手動剖析或建構中繼資料格式。

如果您已經有 [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) 物件代表您想要標記在影像中的位置 (可能從先前使用的地理位置 API 或其他來源)，您可以呼叫 [**GeotagHelper.SetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903685) 並傳入 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 和 **Geopoint**，以設定地理位置標籤資料。

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

若要使用裝置的目前位置來設定地理位置標籤資料，請建立新的 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 物件並呼叫 [**GeotagHelper.SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686)，同時傳入 **Geolocator** 和要標記的檔案。

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   您必須在 app 資訊清單中包含 **location** 裝置功能，才能使用 [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686) API。

-   您必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)，再呼叫 [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686)，以確保使用者已授權您的 app 來使用他們的位置。

-   如需地理位置 API 的詳細資訊，請參閱[地圖與位置](https://msdn.microsoft.com/library/windows/apps/mt219699)。

若要取得代表影像檔地理位置的 GeoPoint，請呼叫 [**GetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903684)。

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## <a name="decode-and-encode-image-metadata"></a>解碼和編碼影像中繼資料

最先進的影像資料處理方式是使用 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 或 [BitmapEncoder](bitmapencoder-options-reference.md)，在資料流層級上讀取及寫入屬性。 對於這些作業，您可以使用 Windows 屬性來指定您要讀取或寫入的資料，但您也可以使用 Windows 影像處理元件 (WIC) 提供的中繼資料查詢語言，指定所要求屬性的路徑。

使用這項技術來讀取影像中繼資料時，您需要有以來源影像檔資料流所建立的 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176)。 相關作法請參閱[影像處理](imaging.md)。

當您有解碼器時，針對您想要擷取的每個中繼資料屬性，請使用 Windows 屬性識別碼字串或 WIC 中繼資料查詢，建立字串清單並加入新項目。 在解碼器的 [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/br226248) 成員上呼叫 [**BitmapPropertiesView.GetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226250) 方法，以要求指定的屬性。 屬性會以機碼/值組的字典傳回，其中包含屬性名稱或路徑及屬性值。

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   如需 WIC 中繼資料查詢語言和支援的屬性的相關資訊，請參閱 [WIC 影像格式原生中繼資料查詢](https://msdn.microsoft.com/library/windows/desktop/ee719904)。

-   許多中繼資料屬性只有在一部分影像類型中才支援。 如果解碼器相關的影像不支援其中一個要求的屬性，[**GetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226250) 會失敗並傳回錯誤碼 0x88982F41，如果影像完全不支援中繼資料，則會傳回 0x88982F81。 這些錯誤碼相關的常數為 WINCODEC\_ERR\_PROPERTYNOTSUPPORTED 和 WINCODEC\_ERR\_UNSUPPORTEDOPERATION，定義於 winerror.h 標頭檔中。
-   因為影像可能包含或不包含特定屬性的值，在嘗試存取屬性之前，請使用 **IDictionary.ContainsKey** 確認屬性存在於結果中。

將影像中繼資料寫入資料流時，需要有影像輸出檔相關聯的 **BitmapEncoder**。

建立 [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) 物件以包含您要設定的屬性值。 建立 [**BitmapTypedValue**](https://msdn.microsoft.com/library/windows/apps/hh700687) 物件以代表屬性值。 這個物件使用 **object** 做為值，並使用 [**PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871) 列舉的成員來定義值的類型。 將 **BitmapTypedValue** 加入到 **BitmapPropertySet**，然後呼叫 [**BitmapProperties.SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252)，使編碼器將屬性寫入資料流。

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   如需哪些影像檔類型支援哪些屬性的詳細資訊，請參閱 [Windows 屬性](https://msdn.microsoft.com/library/windows/desktop/dd561977)、[相片中繼資料原則](https://msdn.microsoft.com/library/windows/desktop/ee872003)和 [WIC 影像格式原生中繼資料查詢](https://msdn.microsoft.com/library/windows/desktop/ee719904)。

-   如果編碼器相關的影像不支援其中一個要求的屬性，[**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) 會失敗並傳回錯誤碼 0x88982F41。

## <a name="related-topics"></a>相關主題

* [影像處理](imaging.md)
 

 




