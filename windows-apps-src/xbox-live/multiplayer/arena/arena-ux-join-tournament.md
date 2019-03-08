---
title: 加入聯賽
description: 了解如何建立供玩家加入您的遊戲聯賽的 UI。
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 競技場，聯賽、 ux
ms.localizationpriority: medium
ms.openlocfilehash: 1cdcfc7243463a35eccfaeb13a3b9b92b616952b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613503"
---
# <a name="join-a-tournament-by-using-the-arena-ui"></a>藉由使用競技場 UI 聯結聯賽

Arena Api 可以提供您註冊，簽入和小組的資訊，但它們的相關資料的標題*不要*提供的功能*執行*相關的工作。 您的標題應該使用競技場 UI 或第三方聯賽組合管理 (TO)，或建置自己的聯賽管理支援。

## <a name="xbox-arena-ui-team-formation"></a>Xbox 競技場 UI:團隊資訊

Arena UI 的玩家到表單或聯結的小組提供的方法。 沒有標題的需求。

###### <a name="ui-example-create-a-team"></a>UI 範例：建立小組

![形成小組畫面](../../images/arena/arena-ux-create-team.png)

#### <a name="when-forming-a-team-a-gamer-can"></a>當組成小組，玩家可以：

* 傳送邀請給加入一個或多個朋友 club 的成員。
* 藉由公佈 LFG 廣告尋找小組成員。
* 登錄或取消註冊 team。
* 移除小組的成員。

## <a name="xbox-arena-ui-registration"></a>Xbox 競技場 UI:註冊

Arena UI 提供一種方法註冊為小組的玩家。 沒有標題的需求。

###### <a name="ui-example-register-a-team"></a>UI 範例：註冊小組

![註冊以團隊螢幕](../../images/arena/arena-ux-register-team.png)

#### <a name="when-registering-for-a-tournament-a-user-can"></a>當您註冊聯賽，使用者可以：

* 移除註冊不再進行聯賽*之前*註冊已關閉。
* 取消註冊小組簽入，或當聯賽正在進行中。
* 取消註冊整個小組。 *個別使用者離開小組取消註冊整個小組的注意。*
* 註冊為 captain。
* 了解聯賽需求以及之前註冊的參與規則。
* 收到註冊成功的通知。
* 請參閱即時變更為 「 註冊 」 的聯賽狀態。
* 預覽聯賽啟動時的方括號。
* 已經註冊了聯賽的球員，請參閱。
* 禁止註冊如果它們不符合錦標賽的資格。
* 禁止註冊如果播放程式已被禁用。

> [!div class="nextstepaction"]
> [比對 engagement](arena-ux-match-engagement.md)
