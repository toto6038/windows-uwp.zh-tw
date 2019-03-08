---
title: 加入您的 Unity 專案中的播放程式統計資料和排行榜
description: 了解如何使用 Xbox Live Unity 外掛程式將播放程式統計資料和排行榜新增至您的 Unity 專案。
ms.assetid: 756b3c31-a459-4ad2-97af-119adcd522b5
ms.date: 10/19/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox 其中 Unity 建立者
ms.localizationpriority: medium
ms.openlocfilehash: 17af9afc8a9048e7222115d2afdc6108d25df1d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658133"
---
# <a name="add-player-stats-and-leaderboards-to-your-unity-project"></a>加入您的 Unity 專案中的播放程式統計資料和排行榜

> [!IMPORTANT]
> Xbox Live Unity 外掛程式不支援的成就或線上多人遊戲，並建議僅用於[Xbox Live 創作者計劃](../developer-program-overview.md)成員。

一旦您加入[Xbox Live 登入](unity-prefabs-and-sign-in.md)到 Unity 專案下, 一個步驟是將播放程式統計資料和排行榜根據這些播放程式統計資料。

具有[Xbox Live Unity 外掛程式](https://github.com/Microsoft/xbox-live-unity-plugin)，您可以輕鬆加入播放程式統計資料和排行榜 Unity 專案中。 類似於登入步驟，您可以選擇使用包含的 prefabs 或附加您自己自訂的遊戲物件包含的指令碼。

## <a name="prerequisites"></a>必要條件
1. [設定 Xbox Live 中 Unity](configure-xbox-live-in-unity.md)
2. [在 Unity 中的登入 Xbox Live](unity-prefabs-and-sign-in.md)

## <a name="player-stats"></a>播放程式統計資料

播放程式 」 狀態是任何有趣的統計資料，您想要追蹤您的玩家。 您追蹤在 Xbox live 的統計資料應該與播放程式中，並以某種方式顯示的統計資料。 這些播放程式統計資料最常用來建置播放程式可以檢視以判斷其效能與其他玩家較量的排列次序的排行榜。 所有的播放程式統計資料會被視為 「 精選統計資料，這表示該遊戲的 GameHub 頁面中，將會顯示統計資料。

播放程式統計資料必須代表單一值。 Xbox Live Unity 外掛程式包含 prefabs 如整數、 雙精確度，以及字串的統計資料。此外，可以延伸到其他資料類型的基底的統計資料物件提供的指令碼。

如需播放程式統計資料的詳細資訊，請參閱[Player stats](../leaderboards-and-stats-2017/player-stats.md)。

> [!NOTE]
> 若要搭配使用播放程式統計資料或排行榜和 Xbox Live 服務，您必須已成功登入的使用者才能存取任何資料。

## <a name="using-the-player-stat-prefabs"></a>使用播放程式 stat prefabs

有數個 prefabs，您可以使用播放程式統計資料與相關的 Xbox Live Unity 外掛程式中提供：

* IntegerStat:可以表示為整數值，例如在一輪的敵人的總數目統計資料。
* DoubleStat:可以表示為浮點數的統計資料點值，例如 kill/死亡比例。
* StringStat:可以表示為字串值時，通常是列舉型別，例如頒一輪的陣序規範，例如 「 黃金 」、 「 銀級 」 或 「 銅牌 」 狀態。
* StatPanel:範例 UI，可用來顯示目前狀態的值。

若要新增播放程式 」 狀態，只需將拖曳 prefab 符合光芒統計資料的資料類型。 在 Unity 的 檢查的狀態，您可以指定三個值：

* 狀態識別碼。這必須符合在合作夥伴中心中設定的識別碼，而且是區分大小寫。
* （此名稱會顯示在 StatPanel prefab UI） 狀態的顯示名稱。
* Stat 場景啟動時的初始值。

您可以使用**StatPanel** prefab 來顯示狀態的值。若要這樣做，請拖曳**StatPanel** prefab 光芒。 您可以指定顯示拖曳至 stat gameobject stat **Stat**欄位**StatPanel** Unity 的偵測器中的物件。

### <a name="manipulating-the-player-stat-values"></a>操作播放程式統計資料值

播放程式統計資料物件具有調整狀態的值，您可以呼叫的函式數目。可以從其他的常式，呼叫這些函式，或繫結至 UI 項目。 您可以看看**DoubleStat**， **IntegerStat**，並**StringStat**即可查看變更的統計資料值的函式的範例指令碼。您可以修改或建立新的指令碼來表示更複雜的函式和邏輯的統計資料。 新的統計資料類別應該擴充`StatBase`中所定義的類別`StatBase`指令碼。

比方說，做為簡單的測試，您可以將 UI 按鈕場景，然後在`OnClick`事件的按鈕，在 Unity 偵測器中新增**IntegerStat**物件，然後呼叫`Increment()`增加值的統計資料的函式一個每次您按一下按鈕。

如果您有也繫結至 stat **StatPanel**物件，您可以看到每次您按一下按鈕更新統計資料值。

每次您更新您的統計資料 （遞增、 遞減等） 時，取得在本機更新的值。 若要有這些統計資料更新會反映在 Xbox Live 必須進行兩件事。 首先，您必須設定其中一個 StatisticManager.SetStatistic 函式的統計資料值。 有三個`StatisticManager`函式以設定統計資料`StatisticManager.SetStatisticIntegerData(XboxLiveUser user, String statName, Int64 value)`， `StatisticManager.SetStatisticNumberData(XboxLiveUser user, String statName, Double value)`，和`StatisticManager.SetStatisticStringData(XboxLiveUser user, String statName, String value)`。 每一個這些函式用來設定您的統計資料的資料類型的適當值。您必須更新您的統計資料的伺服器上，執行的第二件事是*排清*本機資料。 取得排清資料會自動由每隔 5 分鐘`StatManagerComponent`指令碼。  如果您的遊戲結束前 5 分鐘，您必須先確定手動排清資料先確定您不會遺失該進度。 若要這樣做，您必須呼叫`statManagerComponent.RequestFlushToService()`方法，並確定呼叫它的**XboxLiveUser** stat 正在寫入的。

> [!TIP]
> 公認的最佳作法是一律在您的遊戲結束，藉此確定您不會遺失進度之前排清資料。

### <a name="checking-and-verifying-stats"></a>檢查及確認統計資料

`StatisticManager`類別有兩個函式可用於檢查統計資料設定為`XboxLiveUser`，`StatisticManager.GetStatisticNames(XboxLiveUser user)`和`StatisticManager.GetStatistic(XboxLiveUser user, String statName)`。 `GetStatisticNames()` 將提供`list<string>`提供 XboxLiveUser 統計資料名稱來填入。 這些名稱可以用來呼叫統計資料的目前值，可以呼叫`GetStatistic()`函式。 請務必要注意，雖然您可以讀取的 Xbox Live 的統計資料服務中的統計資料不建議，您將它用於遊戲邏輯，而不是要檢查狀態的統計資料後已推送。 服務只為了幫助執行排行榜等其他服務，而不是在遊戲中的統計資料的真實來源。 請務必在標題處理統計資料的所有邏輯，因為不會檢查 Xbox 服務時，都會在您的統計資料，而且它只接受任何值給它的目前狀態為。

## <a name="leaderboards"></a>排行榜

排行榜表示已達成 」 狀態的 「 最佳 」 值的球員的已排序的、 編號清單。比方說，排行榜可能會列出已達到最短的時間在競爭單圈的人，這時玩家可以藉由其他玩家來達成最佳競爭時間其最佳競爭時間。

排行榜根據播放程式統計資料傳送到 Xbox Live 服務的遊戲。 因此，排行榜資料是唯讀的因為您無法直接修改它們。

Xbox Live Unity 外掛程式會提供您可用來了解如何在您的遊戲實作排行榜範例排行榜 prefab。

如需排行榜的詳細資訊，請參閱[排行榜](../leaderboards-and-stats-2017/leaderboards.md)。

## <a name="using-the-leaderboard-prefabs"></a>使用排行榜 prefabs

Xbox Live Unity 外掛程式包含兩個 prefabs 排行榜的：

* 排行榜：物件，代表排行榜，並包含簡單的 UI，以顯示來自排行榜的值。
* LeaderboardEntry:物件，表示單一資料列的排行榜。

您可以拖曳**排行榜**prefab 光芒。 在 Unity 的偵測器，您可以設定下列屬性：

* 狀態：Stat gameobject 此排行榜相關聯。
* 排行榜類型：排行榜項目應該會傳回結果的範圍。
* 項目計數：每頁顯示的資料列數目。

> [!NOTE]
> 排行榜 prefab 的統計資料部分一開始是空白。 嘗試拖曳其中一個狀態的 prefabs 上述到 gameobject 位置進行測試。

在 Unity 編輯器中，**排行榜**prefab 一律會顯示相同的模擬 （mock） 資料，不論偵測器設定為何。 您必須建立並匯出您的專案，Visual Studio，並使用授權的使用者登入。 若要查看實際資料值。 如需詳細資訊，請參閱 < [Unity 中設定 Xbox Live](configure-xbox-live-in-unity.md)。

## <a name="see-also"></a>請參閱

* [在 Unity 中的登入 Xbox Live](unity-prefabs-and-sign-in.md)
* [設定 Xbox Live 中 Unity](configure-xbox-live-in-unity.md)
* [排行榜場景](setup-leaderboard-example-scene.md)
* [取得排行榜資料](unity-leaderboard-from-scratch.md)
