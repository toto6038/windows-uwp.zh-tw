---
title: 顯示人員系統的人員
description: 了解 abou 使用 Xbox Live 人員系統顯示人員的程式碼流程。
ms.assetid: c97b699f-ebc2-4f65-8043-e99cca8cbe0c
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 512de51f2a0e30a9b41a5e49f3dc3ababe30fc4d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658303"
---
# <a name="display-people-from-the-people-system"></a>顯示人員系統的人員

跨 Xbox Live 服務傳回該服務所擁有的唯一資料，並傳回給使用者; 只能 XUID 參考比方說，人們服務只擁有，且會傳回位於使用者的人員清單和某些非常基本的資訊，每一個這些 XUIDs （例如最愛的狀態） 的相關 XUIDs。 「 狀態 」 服務擁有 XUIDs 線上狀態資訊的資料。 排行榜服務擁有 XUIDs 的等級的資訊清單上。 顯示名稱和玩家代號資訊永遠不會傳回從設定檔服務以外的任何服務，因此，呼叫多個服務是為了呈現體驗中的人員清單。

一般的呼叫模式，服務 Api 是對一個往返，先從最適合可篩選的服務中取得一份 XUIDs 或排序清單中，則請同時反覆存取的呼叫來取得任何額外的中繼資料所需的其他服務所需針對每個 XIUD。 在映像的情況下呼叫的第三個往返可能必須取得映像從映像 Url。

若要減少取得使用者的人員清單，使用者的相關資料所需的往返次數*moniker*引進到相關的服務。 這項新功能可讓以抽象方式表示主要服務的服務應人員服務，從取得的使用者的人員清單，並接著將範圍傳回使用 XUIDs 該組的呼叫端。

以下是範例呼叫流程案例，說明項目從與人員相關的服務所取得的資料：

-   目前在遊戲中的使用者清單
-   在線上的目前使用者的人員清單
-   包含隨機使用者的全球排行榜
-   Leaderboard 的使用者的人員


## <a name="list-of-users-currently-in-game"></a>目前在遊戲中的使用者清單

| 有標題  | 目標  | 要呈現的欄位  | 呼叫流程
|-------------------------------------------------|----------------------------------------------------|--------------------|--------------------------------------|
| 隨機 XUIDs 的遊戲中的其他使用者的清單 | 針對每個其他使用者呈現最少的資訊 | GameDisplayName  \[Profile\] | 呼叫 XUIDs 份設定檔。 |


## <a name="list-of-the-current-users-people-who-are-online"></a>在線上的目前使用者的人員清單

## <a name="title-has"></a>標題有：
目前使用者的 XUID

## <a name="goal"></a>目標
若要轉譯的目前使用者的人員清單中的線上使用者豐富的清單

## <a name="field-to-render-owning-service"></a>來呈現欄位\[擁有服務\]
* 最愛的指標 [人員]
* [設定檔] 的顯示圖片
* GameDisplayName [Profile]
* 基本線上狀態 （綠色球形） [存在]

## <a name="call-flow"></a>呼叫流程
1. 呼叫目前狀態，在每個使用者的人員取得 XUIDs 和線上狀態的人員 moniker 中傳遞。
1. 以平行方式：
 1. 呼叫設定檔，在整份 XUIDs 取得顯示名稱，並針對每個圖片的 URL 中傳遞。
 1. 呼叫傳入 XUIDs 找出是否有任何使用者的我的最愛清單的人員。
1. 之後呼叫的設定檔：
 1. 取得映像的每個圖片 URL

## <a name="global-leaderboard-containing-random-users"></a>包含隨機使用者的全球排行榜

## <a name="title-has"></a>標題有：
排行榜識別碼/名稱

## <a name="goal"></a>目標
要呈現的排行榜上每位使用者的基本資訊

## <a name="field-to-render-owning-service"></a>轉譯 [擁有的服務] 欄位
* 最愛的指標 [人員]
* GameDisplayName [Profile]
* 陣序規範 [排行榜]
* 分數 [排行榜]

## <a name="call-flow"></a>呼叫流程
1. 若要取得順位和 XUIDs 呼叫排行榜和特定的排行榜的分數。
1. 以平行方式：
 1. 呼叫設定檔，傳入的 XUIDs 取得顯示名稱，並針對每個圖片的 URL 清單。
 1. 呼叫傳入 XUIDs 找出是否有任何使用者的我的最愛清單的人員。

## <a name="leaderboard-of-users-people"></a>Leaderboard 的使用者的人員

## <a name="title-has"></a>標題有：
* 排行榜識別碼/名稱
* 目前使用者的 XUID

## <a name="goal"></a>目標
要呈現的排行榜上每位使用者的基本資訊

## <a name="field-to-render-owning-service"></a>轉譯 [擁有的服務] 欄位
* 最愛的指標 [人員]
* GameDisplayName [Profile]
* 陣序規範 [排行榜]
* 分數 [排行榜]

## <a name="call-flow"></a>呼叫流程
1. 呼叫使用者的人員清單僅限於特定排行榜的排行榜，人 moniker，以取得 XUIDs 陣序規範、 傳遞和分數。
1. 以平行方式：
 1. 呼叫設定檔，傳入的 XUIDs 取得顯示名稱，並針對每個圖片的 URL 清單。
 1. 呼叫傳入 XUIDs 找出是否有任何使用者的我的最愛清單的人員。
