---
title: GET (/users/{userId}/profile/settings/people/{userList})
assetID: f6553499-89e2-f21b-a00f-7e5437c045ff
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlistget.html
description: " GET (/users/{userId}/profile/settings/people/{userList})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f868fdf4f3d5cd36000784d9c5a3437fa5d67ffa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593853"
---
# <a name="get-usersuseridprofilesettingspeopleuserlist"></a>GET (/users/{userId}/profile/settings/people/{userList})
取得使用者的設定檔，或使用者，以使用者的 Moniker 的支援。 這些 Uri 的網域是`profile.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4EKB)
  * [查詢字串參數](#ID4EVB)
  * [必要的要求標頭](#ID4EQC)
  * [要求本文](#ID4E2D)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
**userList**並**使用者 Id**是互斥的參數。 如果指定兩個或其中一個，您會得到**BadRequest**回。 **userList**是未來技術的兼容性其中多個具名的清單適合用來要求的案例中的陣列。 **使用者 Id**包含十進位字串 XUIDs-JSON 是一點序列化 64 位元不帶正負號的整數。 最後，在 Xbox One 的設定將會命名為設定，使用一般人類看得懂的名稱，而不是 64 位元不帶正負號的整數或難以理解的常數喜歡**XONLINE_PROFILE_ASDF**。
  
<a id="ID4EKB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| userId| 字串| 可以是 'xuid(12345)'、 'gt(myGamertag)，' me'。| 
| userList| 字串| 若要取得設定的人員具名的清單。 目前，人是唯一支援的清單。| 
  
<a id="ID4EVB"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 設定| 字串| 設定名稱的逗號分隔清單。| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 32 位元帶正負號的整數| 值 = 2| 
| content-type| 字串| 值 = <code>application/json</code>| 
  
<a id="ID4E2D"></a>

 
## <a name="request-body"></a>要求本文
 
<a id="ID4EBE"></a>

 
### <a name="sample-request"></a>範例要求
 

```cpp
GET /users/me/profile/settings/people/people?settings=GameDisplayName,GameDisplayPicRaw,Gamerscore,Gamertag
      
```

  
<a id="ID4EKE"></a>

  
 
<a id="ID4EME"></a>

 
##### <a name="response-body"></a>回應主體 
回應**ReadMultiSettingsResponseV2**物件。 假設呼叫的使用者有只有一個 friend:
  

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
                  "value":"http://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F&format=png&w=64&h=64"
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

   
<a id="ID4E3E"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E5E"></a>

 
##### <a name="parent"></a>父系 

[/users/{userId}/profile/settings/people/{userList}?settings={settings}](uri-usersuseridprofilesettingspeopleuserlist.md)

 [設定檔 (JSON)](../../json/json-profile.md)

   