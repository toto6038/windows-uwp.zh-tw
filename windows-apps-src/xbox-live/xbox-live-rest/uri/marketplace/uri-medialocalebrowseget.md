---
title: GET (media/{marketplaceId}/browse)
assetID: 024447a0-c615-e08b-f867-3b6c4c0db5dc
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowseget.html
description: " GET (media/{marketplaceId}/browse)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b072c482fba1f36ac425b98c0126b56e735af078
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622573"
---
# <a name="get-mediamarketplaceidbrowse"></a>GET (media/{marketplaceId}/browse)
可讓瀏覽單一媒體群組內的項目。 這些 Uri 的網域是`eds.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4EFB)
  * [查詢字串參數](#ID4EQB)
  * [回應主體](#ID4E6B)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
此搜尋傳回之資料的頁面可以使用而不使用接續 token skipItems 參數非循序存取。 此 API 會接受查詢 Refiners。 
 
 **SandboxId**現在從 XToken 中的宣告擷取並強制執行。 如果**SandboxId**不存在，則娛樂探索服務 (EDS) 將會擲回 400 不正確的要求時發生錯誤。 
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| marketplaceId| 字串| 必要。 字串值取自<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4EQB"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數
 
此 API 可接受下列的查詢參數： combinedContentRating，desiredMediaItemTypes、 欄位、 maxItems、 preferredProvider、 q、 queryRefiners、 skipItems、 firstPartyOnly、 freeOnly、 hasTrailer、 latestOnly、 subscriptionLevel 和 topRatedOnly.
 
請參閱[EDS 參數](../../additional/edsparameters.md)如需有關這些參數。
  
<a id="ID4E6B"></a>

 
## <a name="response-body"></a>回應主體
 
<a id="ID4EFC"></a>

 
### <a name="sample-response"></a>範例回應
 
下列 JSON 程式碼是以回應呼叫`/media/en-us/browse?orderBy=releaseDate&desiredMediaItemTypes=DGame&fields=all`。
 

```cpp
{
    "Items": [{
        "MediaGroup": "GameType",
        "MediaItemType": "DGame",
        "ID": "6c5402e4-3cd5-4b29-a9c4-bec7d2c7514a",
        "Name": "Tanks-A",
        "ReducedName": "Tanks-A short title",
        "ReleaseDate": "2013-05-14T00: 00: 00Z",
        "TitleId": "1676B413",
        "VuiDisplayName": "Tanks-A vui",
        "DeveloperName": "Microsoft Studios",
        "PublisherName": "Microsoft Studios",
        "SortName": "Tanks-A sort title. ",
        "KValue": "4",
        "KValueNamespace": "bingbox",
        "AllTimePlayCount": 30.0,
        "SevenDaysPlayCount": 30.0,
        "ThirtyDaysPlayCount": 30.0,
        "AllTimeRatingCount": 12.0,
        "ThirtyDaysRatingCount": 12.0,
        "SevenDaysRatingCount": 12.0,
        "AllTimeAverageRating": 0.8,
        "ThirtyDaysAverageRating": 0.8,
        "SevenDaysAverageRating": 0.8,
        "LegacyIds": [{
            "IdType": "TitleId",
            "Value": "1676B413"
        }],
        "Availabilities": [{
            "AvailabilityID": "",
            "ContentId": "c9fcb807-626d-5347-b918-65496f084219",
            "Devices": [{
                "Name": "Durango"
            }]
        }],
        "Packages": [{
            "CdnFileLocation": [{
                "SortOrder": null,
                "Uri": "www.microsoft.com"
            }],
            "ContentId": "c9fcb807-626d-5347-b918-65496f084219",
            "GameRegionMask": 1,
            "PackageType": "CAB",
            "LicenseBit": 1,
            "LicenseType": "SyncCast DTO"
        }],
        "SandboxId": "PART.Dev1"
    },
    {
        "MediaGroup": "GameType",
        "MediaItemType": "DGame",
        "ID": "fd16e2fb-eca4-4182-8f69-a98fdd6e57a1",
        "Name": "Vector based massively multiplayer Tanks game.",
        "ReducedName": "Tanks",
        "ReleaseDate": "2013-03-15T00: 00: 00Z",
        "TitleId": "69A60C76",
        "VuiDisplayName": "Tanks",
        "DeveloperName": "Microsoft Studios",
        "Images": [{
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Box_Art"],
            "Purpose": "Box_Art",
            "Height": 300,
            "Width": 219,
            "Order": 1
        },
        {
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Box_Art"],
            "Purpose": "Box_Art",
            "Height": 120,
            "Width": 85,
            "Order": 1
        },
        {
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Image"],
            "Purpose": "Image",
            "Height": 720,
            "Width": 1280,
            "Order": 1
        }],
        "PublisherName": "Microsoft Studios",
        "RatingId": "10",
        "ParentalRating": "E",
        "ParentalRatingSystem": "ESRB",
        "SortName": "\n            Vector based massively multiplayer Tanks game.\n          ",
        "Capabilities": [{
            "NonLocalizedName": "onlineMultiplayerMin",
            "Value": "3"
        },
        {
            "NonLocalizedName": "onelineMultiplayerMax",
            "Value": "5"
        }],
        "KValue": "5",
        "KValueNamespace": "bingbox",
        "AllTimePlayCount": 30.0,
        "SevenDaysPlayCount": 30.0,
        "ThirtyDaysPlayCount": 30.0,
        "AllTimeRatingCount": 12.0,
        "ThirtyDaysRatingCount": 12.0,
        "SevenDaysRatingCount": 12.0,
        "AllTimeAverageRating": 0.8,
        "ThirtyDaysAverageRating": 0.8,
        "SevenDaysAverageRating": 0.8,
        "LegacyIds": [{
            "IdType": "TitleId",
            "Value": "69A60C76"
        }],
        "Availabilities": [{
            "AvailabilityID": "",
            "ContentId": "7AE9DAB2-1162-488D-9F80-B804EC5AF879",
            "Devices": [{
                "Name": "Durango"
            }]
        }],
        "Packages": [{
            "CdnFileLocation": [{
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            },
            {
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            },
            {
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            }],
            "ContentId": "7AE9DAB2-1162-488D-9F80-B804EC5AF879",
            "PackageType": "XVC",
            "LicenseType": "Xbox Live Games Machine and User"
        }],
        "SandboxId": "PART.Dev1"
    }],
    "ImpressionGuid": "5d3085cf-7d17-43b4-a9c1-a1ccfe764eb1"
}
         
```

   
<a id="ID4EUC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EWC"></a>

 
##### <a name="parent"></a>父系 

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

  
<a id="ID4EAD"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[EDS 常見的標頭](../../additional/edscommonheaders.md)

 [EDS 參數](../../additional/edsparameters.md)

 [EDS 查詢 Refiners](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   