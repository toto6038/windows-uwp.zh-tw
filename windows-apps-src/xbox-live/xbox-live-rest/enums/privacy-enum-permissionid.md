---
title: PermissionId 列舉
assetID: 3e7ee317-4946-1105-ecd7-1e26346deccb
permalink: en-us/docs/xboxlive/rest/privacy-enum-permissionid.html
description: " PermissionId 列舉"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aff463e2268af972536984a00e29348bf0a6859e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594683"
---
# <a name="permissionid-enumeration"></a>PermissionId 列舉
詳細說明 PermissionId 列舉型別。
權限識別碼可以搭配權限驗證 Url:

   * [GET (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

這些識別碼會包含直接檢查針對特定的設定，為使用者，例如檢查單一的隱私權設定的目標或單一權限的動作項目。 此外，有權限，可以搭配 API 的權限，並納入檢查針對特定的使用者動作的多個設定的識別碼。

<a id="ID4EIB"></a>


## <a name="permissions"></a>權限

這些是呼叫端可以用來檢查是否可以執行特定動作的值。 不同於上述的設定，這些封裝服務所定義的原則，並且無法由使用者直接變更，但在其值由使用者定義的一或多個設定上，在大部分情況下，建置原則。 這些是針對上述定義的多個設定通常是複合的檢查。 範例：<b>ViewProfile</b>權限會執行目標的檢查<b>ShareProfile</b>隱私權設定，並要求者<b>AllowProfileViewing</b>權限。

一般情況下，建議您使用呼叫端要求需要檢查，而不是直接檢查 隱私權設定和權限的動作的權限識別碼。 這可讓隱私權原則，以一致的方式跨服務變更，因為會併入新的檢查。

| 權限名稱| 描述|
| --- | --- |
| CommunicateUsingText| 檢查使用者可以將具有文字內容的訊息傳送給目標使用者|
| CommunicateUsingVideo| 請檢查使用者可以通訊使用的目標使用者的影片|
| CommunicateUsingVoice| 請檢查使用者可以通訊使用的目標使用者的語音|
| ViewTargetProfile| 檢查使用者可以檢視目標使用者的設定檔|
| ViewTargetGameHistory| 檢查使用者可以檢視的目標使用者的遊戲歷程記錄|
| ViewTargetVideoHistory| 檢查使用者可以檢視目標使用者的詳細影片看的記錄|
| ViewTargetMusicHistory| 檢查使用者可以檢視詳細的音樂接聽記錄的目標使用者|
| ViewTargetExerciseInfo| 檢查使用者可以檢視的目標使用者的練習資訊|
| ViewTargetPresence| 檢查使用者可以檢視的目標使用者的線上狀態|
| ViewTargetVideoStatus| 檢查使用者可以檢視目標視訊狀態 （線上擴充功能） 的詳細資料|
| ViewTargetMusicStatus| 檢查使用者可以檢視目標音樂狀態 （線上擴充功能） 的詳細資料|
| ViewTargetUserCreatedContent| 檢查使用者可以檢視其他使用者的使用者建立內容|
