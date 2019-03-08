---
title: 設定統計資料和排行榜 2017
description: 了解如何設定在合作夥伴中心使用資料平台 2017年的 Xbox Live 功能統計資料和排行榜。
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ea2baf4bc27e6d1cfd5beb9ef0386acda72a39d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598853"
---
# <a name="configuring-featured-stats-or-leaderboards-in-partner-center-with-data-platform-2017"></a>在合作夥伴中心使用資料平台 2017年設定精選的統計資料或排行榜

使用資料平台 2017 時，您只需要在兩個情況下設定 」 狀態：

* Stat 用於做為基礎的全球排行榜，或
* Stat 是精選的播放程式 」 狀態，將會顯示在 [遊戲中樞] 頁面。

在任一情況下，您必須設定統計資料和排行榜在一起。 每個排行榜根據 」 狀態，，而且您無法設定 」 狀態而不需要也設定相關聯的全球排行榜，即使您不打算排行榜用於精選的播放程式 」 狀態。

您不需要設定統計資料不精選的播放程式統計資料，且不會使用的全球排行榜。

## <a name="configure-a-global-leaderboard-and-an-associated-player-stat"></a>設定的全球排行榜和相關聯的播放程式統計資料

首先，移至標題的播放程式統計資料 區段，如下所示。

![](../images/omega/dev_center_player_stats_creators.png)

然後按一下 `Create your first featured stat/leaderboard`按鈕，並填寫此對話方塊，然後按一下 儲存。

![](../images/omega/dev_center_player_stats_creators_leaderboard.png)

`Display name`欄位便是使用者會看到 GameHub 中。  這個字串中可當地語系化`Localize strings`一節。  按一下 `Show Options`在`Localize strings`一節，以查看如何以當地語系化這些字串的詳細資料。

`ID`欄位的統計資料的名稱，以及將如何您將參考您的統計資料更新從標題的程式碼時。   請參閱[更新的統計資料](player-stats-updating.md)如需詳細資訊，如下一節。

`Format`是統計資料的資料格式。

`Display Logic`提供之間的抉擇`Player progression`， `Personal best`，和`Counter`:
- 播放程式進展：代表個別的播放程式層級或遊戲中的進展。  最後一個設定的值是使用者會看到的內容。  比方說，放在目前的往返。
- 個人最佳：表示目前播放程式已發佈的最佳分數。 根據排序次序所設定的最大或最小值是使用者會看到的內容。  例如，最快單圈時間。
- 計數器:可以加入其他玩家的來計算累計數目。  

`Sort`欄位可讓您變更排行榜的排序次序。

您也可以選擇讓此 stat `Featured Stat`，但按一下`Feature on players' profiles`。  

## <a name="configure-featured-stats"></a>設定精選的統計資料

當您定義播放程式 」 狀態時，您可以選擇檢查`Featured Stat`。  請注意下列需求

| 開發人員類型 | 需求 |
|----------------|-------------|
| Xbox Live 創作者計畫 | 沒有將任何統計資料指定為精選的統計資料的需求。雖然您受限於最多 10 個 |
| ID@Xbox 與 Microsoft 合作夥伴 | 您必須指定介於 3 到 10 個精選的統計資料 |

## <a name="next-steps"></a>後續步驟

接下來您將需要更新從標題的程式碼狀態。  請參閱[更新的統計資料](player-stats-updating.md)如需詳細資訊。