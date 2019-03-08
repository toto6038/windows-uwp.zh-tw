---
title: 設計即時的 Xbox 體驗
description: 了解如何規劃出玩家統計資料、 排行榜及標題的成就 Xbox Live 成員設計絕佳的體驗。
ms.assetid: d2a85621-7d02-4b11-a7fa-9ebaee0092d5
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 統計資料、 成就、 排行榜、 設計
ms.localizationpriority: medium
ms.openlocfilehash: 0f080593727ec6d7ddd529b2ce976708174250ba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600783"
---
# <a name="designing-xbox-live-experiences"></a>設計即時的 Xbox 體驗

本主題會引導您思考，並加入使用者統計資料、 成積、 和排行榜對您的遊戲使用 Xbox Live 的資料平台功能的範例程序。

## <a name="example"></a>範例
讓我們設定一個虛構的遊戲，顯示的資料平台如何協助您亮起令人讚嘆的 Xbox Live 體驗在 Xbox One 儀表板，在 Windows 10 上的 Xbox 應用程式和遊戲中。 此案例中，我們將使用賽車遊戲虛_競爭情況 2015年_。

若要取得大部分的值，超出這些體驗，我們會遵循這些建議的規劃階段：
1. 設計 XBL 經驗
2. 設計強化您的案例所需的統計資料
3. 視需要設定精選的統計資料、 成積、 或排行榜


## <a name="1-design-your-xbox-live-experiences"></a>1.設計您的 Xbox Live 的經驗
我們希望_競爭 2015年_來取得充分利用 Xbox Live，以讓使用者參與內部和外部的遊戲。 第一個問題是：

1. 怎麼做我們想要讓它像我們 GameHub 成就 索引標籤的體驗？ （精選的統計資料和社交排行榜）
2. 哪些目標與推動我們想要提供給玩家的遊戲中？ （成就）
3. 分數或統計資料為何我們想用來排列本身與其他玩家較量遊戲中的播放程式？ （排行榜）


### <a name="gamehubs---featured-statistics-and-social-leaderboards"></a>GameHubs-精選的統計資料和社交排行榜
GameHubs 包括登陸經驗的使用者可以了解關於遊戲的所有項目。 為開發人員和/或 「 發行者 」 所使用，這是最好的地方，以便_showcase_遊戲如何絕佳且豐富的 Xbox 使用者還不購買遊戲。 GameHubs 也被設計來顯示進度和吸引人的計量玩家已擁有標題中。 底下的成就 索引標籤中，使用者可以尋找遊戲進度 & 成就，Hero Stats 和社交排行榜。

本節的目標是要擷取遊戲最吸引人的計量，並將它們與遊戲提供唯一的播放程式經驗的圖片，並進行排名，對他們朋友共享的排行榜中公開。 比方說，Forza 6 成就 索引標籤可能看起來像這樣：

![Gamehub 映像](../images/omega/forza_gamehub.png)


您會發現一些統計資料，例如英哩驅動和第一個位置完成擁有金、 銀級，或銅牌說明其中播放程式，將對他的朋友們會排列次序的裝飾。 這些統計資料的任何互動，將會顯示完整的排行榜，就像這樣：

![排行榜](../images/omega/progress_gamehub_lb.png)

 針對_競爭 2015年_，我們相信，以下設定統計資料，代表標題的玩家的體驗，以及很好的良好排名計量：
 * 最快單圈時間
 * 最第 1 個地方 wins
 * 行駛


### <a name="achievements"></a>成就
成就都將導向和使用者的遊戲中動作獎勵以一致的方式，跨所有遊戲的全系統的機制。 這在正確設計可協助指引使用者體驗在其發揮最大的遊戲，並延長存留期的遊戲。

針對_競爭情況 2015年_，以下是我們想要設定的成就的子集：
* 在 60 秒內完成 1 單圈時間
* 完成第 1 名的 10 個競爭情況
* 完成至少 1 個競賽在每個資料軌
* 5 個汽車
* 10,000 英哩的磁碟機
* ...


###  <a name="leaderboards"></a>排行榜
排行榜提供玩家評分本身對其他玩家在遊戲中可以達成的特定動作的方式。 除了在 GameHubs 社交排行榜，我們可以設定要用於遊戲的全球排行榜。 以下是一些我們會想要我們所有的使用者排名在排行榜：

* 最快單圈時間
 * 這在何處發生的中繼資料： 追蹤
 * 使用的中繼資料： 汽車
 * 中繼資料： 天氣狀況
* 最第 1 個地方 wins
* 大部分的車輛收集

## <a name="next-steps"></a>後續步驟
既然您已經知道如何有效地設計統計資料、 排行榜和成就，您現在可以開始開始實作這些功能的標題。  接下來的章節將說明從合作夥伴中心中設定的端對端程序。

請參閱[播放程式統計資料](../leaderboards-and-stats-2017/player-stats.md)
