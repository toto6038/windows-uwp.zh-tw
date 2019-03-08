---
title: Xbox Live 的資料平台
description: Xbox Live 資料平台，其中包含服務管理成積、 玩家統計資料和排行榜的概觀。
ms.assetid: a8bb7c4f-09fe-4dba-b3ce-1fab60453831
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 統計資料、 成就、 排行榜、 資料平台
ms.localizationpriority: medium
ms.openlocfilehash: f9a14c508e265cdfc510896eb621466d03c66776
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634833"
---
# <a name="xbox-live-data-platform---stats-leaderboards-achievements"></a>Xbox Live 資料平台-Stats 排行榜成就

Xbox Live 資料平台撰寫遊戲資料，可讓您以服務方式執行的標題。 此外，Xbox Live 的資料平台驅動使用者參與，以使用統計資料、 排行榜及成積、 標題和呈現功能的主控台介面和 Xbox 應用程式中的統計資料。

賽車遊戲狀態的其中一個範例可能是總計的競爭情況在拖曳帶狀模式下執行，這可用來建立比較您對社群的得分排行榜，而且可以反映在即時豐富存在字串中 （例如"GoTeamEmily 播放拖曳帶狀模式：523 競爭情況已完成 」）。 總計的競爭情況，在拖曳區模式也可用之準則的配對，確保播放程式使用類似的體驗會更容易在一起的比對。

您的標題可以顯示播放程式統計資料為基礎的排行榜。比方說，排行榜可能全域排名的已完成的競爭情況。 呼叫 Xbox Live Api 直接使用，這些服務或遊戲引擎的包裝函式例如 Unity。

您可以指定特定統計資料為精選的統計資料。精選的統計資料會顯示在您的標題 GameHub，並比較針對他們朋友的播放程式。

成就不提供的統計資料，和您的標題會決定當成就已解除鎖定。 比方說您的標題可能必須完成下一分鐘的競爭的成就。 您的標題會追蹤的解除鎖定成就所需的參數。 在此範例中會啟動您的標題，來測量時間競爭花了，然後獎品成就。 一般而言，玩家分數會獲頒獎項以及完成的成果。 它可以決定要決定每個成就的玩家分數的金額。

## <a name="features"></a>功能 ##
下列主題將提供 Xbox Live 的資料平台功能的詳細資訊：

* [播放程式統計資料](../leaderboards-and-stats-2017/player-stats.md)
* [成就](../achievements-2017/achievements.md)
* [排行榜](../leaderboards-and-stats-2017/leaderboards.md)
