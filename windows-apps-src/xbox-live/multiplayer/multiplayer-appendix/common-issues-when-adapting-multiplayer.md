---
title: 常見的多人遊戲 2015年移轉問題
description: 深入了解調整到 2015年的多人在多人遊戲 2014年標題時可能遇到的常見問題。
ms.assetid: 206f8fe4-c7aa-44b8-923b-18f679d8439f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a04370ee2f45534c88467700b9523c5a4ad11094
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615813"
---
# <a name="common-issues-when-adapting-your-multiplayer-2014-title-to-multiplayer-2015"></a>常見的問題時調整多人遊戲 2014年標題多人遊戲 2015

本主題描述針對 2015年多人採用做法您 2014年的多人連線遊戲時，您必須考慮的問題。


## <a name="configuration-changes-to-make-for-2015-multiplayer"></a>2015 多人遊戲的進行的設定變更

本章節描述時要注意的 2015年多人遊戲設定您的工作階段和範本的變更。 例如特定的項目所討論的詳細資訊，請參閱[MPSD 工作階段範本](multiplayer-session-directory.md)。

### <a name="add-a-capability-for-active-member-connection"></a>新增一項功能的作用中的成員連接

ConnectionRequiredForActiveMembers 功能可讓您中斷連線偵測和工作階段變更訂用帳戶的 2015年多人遊戲的功能。 所有的工作階段範本 /constants/system/capabilities 物件中加入這項功能。


### <a name="add-a-system-constant-for-invite-protocol"></a>將系統常數加入邀請的通訊協定

InviteProtocol 系統常數，可讓寄件者的標題會呼叫時，接收快顯通知邀請的收件者**MultiplayerService.SendInvitesAsync 方法**或**SystemUI.ShowSendGameInvitesAsync方法 (IUser，IMultiplayerSessionReference，String)**。 加入這個常數，設定為 「 遊戲 」，工作階段的標題會邀請播放程式的所有範本的 /constants/system 物件。


## <a name="runtime-considerations-for-2015-multiplayer"></a>2015 多人遊戲的執行階段的考量

適用於 2015年標題多人必須： 請務必呼叫**MultiplayerService.EnableMultiplayerSubscriptions 方法**之前輸入標題的程式碼的多人遊戲區域。 此呼叫可讓工作階段變更這兩個訂用帳戶，並中斷連線偵測。
-   請務必使用相同**XboxLiveContext 類別**由同一位使用者的所有呼叫的物件。 內容包含多人遊戲的訂用帳戶所使用的連接管理相關的狀態，並中斷連線偵測。
-   如果有多個本機使用者，使用個別**XboxLiveContext**每位使用者的物件。


## <a name="migrating-a-session-template-from-contract-version-104105-to-107"></a>從合約版本 104/105 的工作階段範本移轉至第 107

最新的工作階段範本合約版本是 107，與用於 MPSD 的結構描述的某些變更。 如果它們會重新發佈至使用版本 107，則必須更新您已撰寫的工作階段範本合約版本 104/105 的範本。 本主題摘要說明要將您的範本移轉為最新版本進行的變更。 範本本身所述[MPSD 工作階段範本](multiplayer-session-directory.md)。

| 重要事項                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 透過範本設定的功能不能透過變更寫入至 MPSD。 若要變更的值，您必須建立並提交新的範本，內含所需的變更。 透過範本未設定任何項目可透過寫入變更為 MPSD。 |


### <a name="changes-to-the-constantssystemmanagedinitialization-object"></a>/Constants/system/managedInitialization 物件的變更

/Constants/system/managedInitialization 物件已更名為 /constants/system/memberInitialization。 以下是要對此物件的名稱/值組的變更： autoEvaluate 已重新命名為 externalEvaluation。 它的極性變更，除了 false 會保持預設值。
-   為 1，從 2 變更 membersNeededToStart 的預設值。
-   為 10 秒，從 5 秒變更 joinTimeout 的預設值。
-   MeasurementTimeout 的預設值為 30 秒變更從 5 秒。


### <a name="changes-to-the-constantssystemtimeouts-object"></a>/Constants/system/timeouts 物件的變更

會移除 /constants/system/timeouts 物件，而逾時是重新命名，並在 /constants/system 重新放置。 以下是對名稱/值組，這個物件進行變更： 保留的逾時，會變成 reservedRemovalTimeout。
-   非使用中的逾時，會變成 inactiveRemovalTimeout。 其新的預設值為 0 （小時）。
-   準備好的逾時變成 readyRemovalTimeout。
-   SessionEmpty 逾時，會變成 sessionEmptyTimeout。

逾時值的詳細資料會出現在[工作階段逾時](mpsd-session-details.md)。


### <a name="change-to-the-game-play-capability"></a>遊戲功能變更

遊戲播放功能可讓新的玩家和評價報告。 現在，您必須指定為 true /constants/system/capabilities/gameplay 欄位，以表示實際的遊戲進行，而不是協助程式的工作階段，比方說，比對和大廳工作階段的工作階段。


## <a name="see-also"></a>請參閱

[MPSD 工作階段範本](mpsd-session-details.md)
