---
title: POST (/users/batch/profile/settings)
assetID: 2a619148-a626-f413-bda1-a2790063075d
permalink: en-us/docs/xboxlive/rest/uri-usersbatchprofilesettingspost.html
description: " POST (/users/batch/profile/settings)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0f859a58e32624223d59d918d46f6230a3abd6db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662223"
---
# <a name="post-usersbatchprofilesettings"></a>POST (/users/batch/profile/settings)
取得一或多位使用者的設定檔。 這些 Uri 的網域是`profile.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [Authorization](#ID4EFB)
  * [必要的要求標頭](#ID4EOB)
  * [要求本文](#ID4EZC)
  * [回應主體](#ID4EJD)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
這是只有完整的設定檔 URL，而此一中允許。 所有其他設定檔 Api，從用戶端會遭到封鎖。
  
<a id="ID4EFB"></a>

 
## <a name="authorization"></a>Authorization
 
若要存取設定檔，只有一般驗證權杖和宣告所需。
  
<a id="ID4EOB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | 
| x-xbl-contract-version| 32 位元不帶正負號的整數| 合約版本必須設為 2，區別這個從 Xbox 360 API 的呼叫。| 
| content-type| 字串| 值 = <code>application/json</code>| 
  
<a id="ID4EZC"></a>

 
## <a name="request-body"></a>要求本文
 
<a id="ID4E6C"></a>

 
### <a name="sample-request"></a>範例要求
 

```cpp
POST /users/batch/profile/settings
   {
      "userIds":[
         "2533274791381930"
       ],
      "settings":[
         "GameDisplayName",
         "GameDisplayPicRaw",
         "Gamerscore",
         "Gamertag"
      ]
   }
      
```

   
<a id="ID4EJD"></a>

 
## <a name="response-body"></a>回應主體
 
<a id="ID4EPD"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
      "profileUsers":[
         {
            "id":"2533274791381930",
            "settings":[
               {
                  "id":"GameDisplayName",
                  "value":"John Smith"
               },
               {
                  "id":"GameDisplayPicRaw",
                  "value":"https://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F"
               },
               {
                  "id":"Gamerscore",
                  "value":"0"
               },
               {
                  "id":"Gamertag",
                  "value":"CracklierJewel9"
               }
            ]
         }
      ]
   }
         
```

   
<a id="ID4EZD"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E2D"></a>

 
##### <a name="parent"></a>父系 

[/users/batch/profile/settings](uri-usersbatchprofilesettings.md)

  
<a id="ID4EFE"></a>

 
##### <a name="reference"></a>參考 

[設定檔 (JSON)](../../json/json-profile.md)

   