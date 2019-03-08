---
title: JavaScript 物件標記法 (JSON) 物件參考
assetID: 8efcc6f3-d88a-1b15-bcfc-d79a24581b0a
permalink: en-us/docs/xboxlive/rest/atoc-xboxlivews-reference-json.html
description: " JavaScript 物件標記法 (JSON) 物件參考"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c46557e3fb837bebccbb1039fb416f3e9787af2a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626553"
---
# <a name="javascript-object-notation-json-object-reference"></a>JavaScript 物件標記法 (JSON) 物件參考
 
JavaScript Object Notation (JSON) 是輕量型、 標準式、 物件導向的標記法，來封裝在網站上的資料。
 
Xbox Live 服務會定義所使用的 JSON 物件中的要求及從服務的回應。 本節提供有關使用使用 Xbox Live 服務的每個 JSON 物件的參考資訊。
 
<a id="ID4EHB"></a>

 
## <a name="in-this-section"></a>本節內容

[成就 (JSON)](json-achievementv2.md)

&nbsp;&nbsp;成就物件 （第 2 版）。

[ActivityRecord (JSON)](json-activityrecord.md)

&nbsp;&nbsp;一或多個使用者的完整的目前狀態的相關格式化和當地語系化字串。

[ActivityRequest (JSON)](json-activityrequest.md)

&nbsp;&nbsp;要求一或多個使用者的完整的目前狀態的相關資訊。

[AggregateSessionsResponse (JSON)](json-aggregatesessionsresponse.md)

&nbsp;&nbsp;包含使用者的適用性工作階段的彙總的資料。

[BatchRequest (JSON)](json-batchrequest.md)

&nbsp;&nbsp;用來篩選顯示狀態資訊，例如使用者、 裝置和標題的屬性陣列。

[DeviceEndpoint (JSON)](json-deviceendpoint.md)

[DeviceRecord (JSON)](json-devicerecord.md)

&nbsp;&nbsp;裝置，包括其型別和它的作用中的項目相關的資訊。

[Feedback (JSON)](json-feedback.md)

&nbsp;&nbsp;包含有關播放程式的意見反應資訊。

[GameClip (JSON)](json-gameclip.md)

[GameClipsServiceErrorResponse (JSON)](json-gameclipsserviceerrorresponse.md)

&nbsp;&nbsp;/Users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} 回應的選擇性部分/uri/格式 / {gameClipUriType} API。

[GameClipThumbnail (JSON)](json-gameclipthumbnail.md)

&nbsp;&nbsp;包含個別的縮圖的相關資訊。 可以有多個大小，每個剪輯，並由用戶端選取適當的顯示。

[GameClipUri (JSON)](json-gameclipuri.md)

[GameMessage (JSON)](json-gamemessage.md)

&nbsp;&nbsp;JSON 物件，在遊戲工作階段的訊息佇列中定義之訊息的資料。

[GameResult (JSON)](json-gameresult.md)

&nbsp;&nbsp;JSON 物件，表示描述的遊戲工作階段結果的資料。

[GameSession (JSON)](json-gamesession.md)

&nbsp;&nbsp;JSON 物件，代表遊戲資料的多人連線的工作階段。

[GameSessionSummary (JSON)](json-gamesessionsummary.md)

&nbsp;&nbsp;JSON 物件，代表遊戲的工作階段的摘要資料。

[GetClipResponse (JSON)](json-getclipresponse.md)

&nbsp;&nbsp;包裝的遊戲的剪輯。

[HopperStatsResults (JSON)](json-hopperstatsresults.md)

&nbsp;&nbsp;JSON 物件，代表 hopper 的統計資料。

[InitialUploadRequest (JSON)](json-initialuploadrequest.md)

&nbsp;&nbsp;主體的 POST 遊戲剪輯上傳要求。

[InitialUploadResponse (JSON)](json-initialuploadresponse.md)

[inventoryItem (JSON)](json-inventoryitem.md)

&nbsp;&nbsp;Core 清查項目所表示的權限可以授與的標準項目。

[LastSeenRecord (JSON)](json-lastseenrecord.md)

&nbsp;&nbsp;當系統最後看到的使用者，使用者有無有效 DeviceRecord 時可供使用的相關資訊。

[MatchTicket (JSON)](json-matchticket.md)

&nbsp;&nbsp;JSON 物件，代表符合票證，用來找出透過多人連線的工作階段目錄 (MPSD) 的其他播放程式播放程式。

[MediaAsset (JSON)](json-mediaasset.md)

&nbsp;&nbsp;成就或照樣相關聯之媒體資產。

[MediaRecord (JSON)](json-mediarecord.md)

[MediaRequest (JSON)](json-mediarequest.md)

[MultiplayerActivityDetails (JSON)](json-multiplayeractivitydetails.md)

&nbsp;&nbsp;JSON 物件，表示**Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**。

[MultiplayerSessionReference (JSON)](json-multiplayersessionreference.md)

&nbsp;&nbsp;JSON 物件，表示**MultiplayerSessionReference**。 

[MultiplayerSessionRequest (JSON)](json-multiplayersessionrequest.md)

&nbsp;&nbsp;要求的 JSON 物件上作業所傳遞**MultiplayerSession**物件。

[MultiplayerSession (JSON)](json-multiplayersession.md)

&nbsp;&nbsp;JSON 物件，表示**MultiplayerSession**。 

[PagingInfo (JSON)](json-paginginfo.md)

&nbsp;&nbsp;包含資料頁面中傳回結果的分頁的資訊。

[PeopleList (JSON)](json-peoplelist.md)

&nbsp;&nbsp;集合[人員](json-person.md)物件。

[PermissionCheckBatchRequest (JSON)](json-permissioncheckbatchrequest.md)

&nbsp;&nbsp;PermissionCheckBatchRequest 物件的集合。

[PermissionCheckBatchResponse (JSON)](json-permissioncheckbatchresponse.md)

&nbsp;&nbsp;結果的批次的權限檢查有多個使用者的權限值的清單。

[PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)

&nbsp;&nbsp;原因的批次的權限檢查的單一目標使用者的權限值清單。

[PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)

&nbsp;&nbsp;從單一的權限設定，針對單一的目標使用者的單一使用者檢查的結果。

[PermissionCheckResult (JSON)](json-permissioncheckresult.md)

&nbsp;&nbsp;從單一的權限設定，針對單一的目標使用者的單一使用者檢查的結果。

[人員 (JSON)](json-person.md)

&nbsp;&nbsp;人員系統中的單一個人相關的中繼資料。

[PersonSummary (JSON)](json-personsummary.md)

&nbsp;&nbsp;集合[人員 (JSON)](json-person.md)物件。

[播放程式 (JSON)](json-player.md)

&nbsp;&nbsp;包含播放程式的資料，在遊戲工作階段中。

[PresenceRecord (JSON)](json-presencerecord.md)

&nbsp;&nbsp;單一使用者的線上的目前狀態的相關資料。

[設定檔 (JSON)](json-profile.md)

&nbsp;&nbsp;使用者的個人設定檔設定。

[進展 (JSON)](json-progression.md)

&nbsp;&nbsp;使用者的進展，朝解除鎖定成就。

[屬性 (JSON)](json-property.md)

&nbsp;&nbsp;包含用戶端所提供的配對要求準則的屬性資料。

[QueryClipsResponse (JSON)](json-queryclipsresponse.md)

&nbsp;&nbsp;會包裝傳回遊戲的剪輯，以及分頁資訊清單的清單。

[quotaInfo (JSON)](json-quota.md)

&nbsp;&nbsp;包含標題群組的配額資訊。

[需求 (JSON)](json-requirement.md)

&nbsp;&nbsp;解除鎖定的成就和伸展多遠的使用者是朝向符合這些準則。

[ResetReputation (JSON)](json-resetreputation.md)

&nbsp;&nbsp;包含新基底應該要變更使用者的現有分數的信譽分數。

[Reward (JSON)](json-reward.md)

&nbsp;&nbsp;成就與相關聯的報酬。

[RichPresenceRequest (JSON)](json-richpresencerequest.md)

&nbsp;&nbsp;如需應該使用豐富的目前狀態資訊的詳細資訊的要求。

[ServiceError (JSON)](json-serviceerror.md)

&nbsp;&nbsp;包含當服務呼叫失敗時傳回錯誤的相關資訊。

[ServiceErrorResponse (JSON)](json-serviceerrorresponse.md)

&nbsp;&nbsp;發生服務錯誤時，就會傳回適當的 HTTP 錯誤碼。 （選擇性） 此服務也可能包括 ServiceErrorResponse 物件，定義如下。 在生產環境中可能包含較少的資料。

[SessionEntry (JSON)](json-sessionentry.md)

&nbsp;&nbsp;包含符合工作階段資料。

[TitleAssociation (JSON)](json-titleassociation.md)

&nbsp;&nbsp;成就與相關聯的標題。

[TitleBlob (JSON)](json-titleblob.md)

&nbsp;&nbsp;包含標題，以從儲存體的相關資訊。

[TitleRecord (JSON)](json-titlerecord.md)

&nbsp;&nbsp;標題，包括其名稱和上次修改時間戳記的相關資訊。

[TitleRequest (JSON)](json-titlerequest.md)

&nbsp;&nbsp;標題的相關資訊的要求。

[UpdateMetadataRequest (JSON)](json-updatemetadatarequest.md)

&nbsp;&nbsp;應該針對剪輯更新中繼資料。

[使用者 (JSON)](json-user.md)

&nbsp;&nbsp;包含使用者排行榜資料。

[UserClaims (JSON)](json-userclaims.md)

&nbsp;&nbsp;傳回目前已驗證使用者的相關資訊。

[UserList (JSON)](json-userlist.md)

&nbsp;&nbsp;集合[使用者](json-user.md)物件。

[UserSettings (JSON)](json-usersettings.md)

&nbsp;&nbsp;傳回目前已驗證使用者的設定。

[UserTitle (JSON)](json-usertitlev2.md)

&nbsp;&nbsp;包含使用者項目資料。

[VerifyStringResult (JSON)](json-verifystringresult.md)

&nbsp;&nbsp;對應到送出至每個字串的程式碼會造成[/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md)。

[XuidList (JSON)](json-xuidlist.md)

&nbsp;&nbsp;在其上執行作業的 XUIDs 清單。
 
<a id="ID4ENH"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EPH"></a>

 
##### <a name="parent"></a>父系 

[Xbox Live 服務符合 rest 限制的參考](../atoc-xboxlivews-reference.md)

  
<a id="ID4EZH"></a>

 
##### <a name="external-links-ecma-international-standard-262-ecmascript-language-specificationhttpswwwecma-internationalorgpublicationsfilesecma-stecma-262pdf"></a>外部連結[ECMA International Standard 262:ECMAScript 語言規格](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-262.pdf)

   