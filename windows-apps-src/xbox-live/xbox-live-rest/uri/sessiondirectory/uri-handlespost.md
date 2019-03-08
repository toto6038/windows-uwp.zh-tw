---
title: POST (/handles)
assetID: 21f3e289-0b0e-2731-befb-bd4c0d71973e
permalink: en-us/docs/xboxlive/rest/uri-handlespost.html
description: " POST (/handles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ed3482b8e629749d294ed25944db16372cc7fee6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594743"
---
# <a name="post-handles"></a>POST (/handles)
設定使用者的目前活動的多人遊戲工作階段，並邀請工作階段的成員，如有必要。

> [!IMPORTANT]
> 這個方法由 2015年多人遊戲，並只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本，搭配使用，而且需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4EHB)
  * [HTTP 狀態碼](#ID4EPB)
  * [要求本文](#ID4EVB)
  * [回應主體](#ID4EJC)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註

此 HTTP/REST 方法可用來設定目前活動的工作階段。 在此情況下，此方法可以由包裝**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SetActivityAsync**。 要求本文必須定義工作階段參考，請使用**sessionRef** JSON 檔案，以 「 活動 」 的 [類型] 欄位中的物件。 會不擷取任何回應本文。 如需工作階段的參考中指定的項目定義，請參閱**Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference**。

這個 POST 方法也可用來邀請使用者的工作階段控制代碼所指定。 在此情況下，此方法可以由包裝**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SendInvitesAsync**。 這種使用 POST 方法需要您的要求本文，來定義工作階段的參考，但與型別欄位設定為 [邀請]。 回應主體是邀請控制代碼。

<a id="ID4EHB"></a>


## <a name="uri-parameters"></a>URI 參數

無

<a id="ID4EPB"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EVB"></a>


## <a name="request-body"></a>要求本文

<a id="ID4E1B"></a>


### <a name="request-body-for-setting-activity"></a>要求本文來設定活動


```cpp
{
  "version": 1,
  "type": "activity",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    // The session name is optional in a POST; if not specified, MPSD fills in a GUID.//
    "name": "session-49"
  },
}

```


<a id="ID4EBC"></a>


### <a name="request-body-for-sending-invites"></a>要求主體傳送邀請


```cpp
{
  // Common handle fields
  "id: "47ca0049-a5ba-4bc1-aa22-fcf834ce4c13",
  "version": 1,
  "type": "invite",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    "name": "session-49"
   },
   "inviteAttributes": {
     "titleId" : {titleId}, // The title being invited to, in decimal uint32. This value is used to find the title name and/or image.
     "context" : {context}, // The title defined context token. Must be 256 characters or less when URI-encoded.
     "contextString" : {contextstring}, // The string name of a custom invite string to display in the invite notification.
     "senderString" : {sender} // The string name of the sender when the sender is a service.
   },
   "invitedXuid": "3210",
}

```


<a id="ID4EJC"></a>


## <a name="response-body"></a>回應主體

<a id="ID4EOC"></a>


### <a name="response-body-for-setting-activity"></a>設定活動的回應主體
無。  
<a id="ID4ESC"></a>


### <a name="response-body-for-sending-invites"></a>傳送邀請的回應主體
邀請控制代碼。   
<a id="ID4EXC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EZC"></a>


##### <a name="parent"></a>父系

[/handles](uri-handles.md)
