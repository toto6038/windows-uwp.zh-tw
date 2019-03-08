---
title: 更新統計資料 2017
description: 了解如何更新使用統計資料 2017年的 Xbox Live player 統計資料。
ms.assetid: 019723e9-4c36-4059-9377-4a191c8b8775
ms.date: 08/24/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 播放程式統計資料，統計資料 2017
ms.localizationpriority: medium
ms.openlocfilehash: d5b37c008e6aa719b641321c5e5a1c3360b20786
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631823"
---
# <a name="updating-stats-2017"></a>更新統計資料 2017

您傳送 Xbox Live 服務使用的最新值來更新統計資料`StatsManager`將下面討論的 Api。

是由您的標題，來追蹤玩家統計資料，而且您呼叫`StatsManager`適當地更新。  `StatsManager` 將緩衝區的任何變更，這些服務會定期排清。  您的標題可以手動排清。

> [!NOTE]
> 沒有太頻繁清除統計資料。  否則您的標題會速率限制。  最佳的作法是清除一次最多每隔 5 分鐘。

### <a name="multiple-devices"></a>多個裝置

您有多個裝置上播放您的標題的播放程式。  在此情況下，您需要進行特定工作，讓項目保持同步。

例如，如果玩家有 15 致勝在家其 Xbox 上。  更新版本中，當他們開始 10 的多個致勝其 Xbox 上在朋友的房子。  您必須將第二個裝置上的統計資料的值為 25。  但您會有沒有辦法知道這一點之後，而不需要以某種方式同步處理這項資訊。

有幾種方式執行這項操作：

1. 將它們儲存使用[連接的儲存體](../storage-platform/connected-storage/connected-storage-technical-overview.md)。  通常您會使用連接儲存體，每個使用者儲存的資料。  這項資料在指定使用者的不同裝置上保持同步。
2. 使用您自己的 web 服務，讓統計資料保持同步，如果您已經有另一個用於執行輔助工作標題。

### <a name="offline"></a>離線

如我們先前所述，您的標題負責追蹤的播放程式統計資料，因此，responsbile 支援離線情況。 

### <a name="examples"></a>範例

我們將探討結合這些概念的範例。

賽車遊戲中的常見狀態是單圈時間。  通常較低適合這些統計資料。因此您會建立統計資料和相關聯的排行榜，其中較低是較佳。  換句話說，會以遞增順序排序此排行榜。

您的標題會追蹤的使用者的單圈時間，在他們的播放工作階段。  只有他們具有低於其先前的最佳單圈時間，您會更新統計資料管理員。

您可以使用下列方法之一來追蹤其先前的最佳：
1. 從儲存檔案中，使用連接儲存體。
2. 您自己的 web 服務。

服務將會取代任何狀態的值。  因此即使您使用大於其前一個最佳的單圈時間來更新，然後其先前的最佳會被覆寫。

因此請在您的標題，確定您只會傳送適當的統計資料值，根據您的遊戲案例。  在某些情況下，較低的值可能是更好、 輸入某些其他情況下更高版本可能會更好的或其他項目完全。

## <a name="programming-guide"></a>程式設計手冊

通常會使用統計資料的流程：

1. 初始化`StatsManager`藉由傳入的本機使用者的 API。
1. 當使用者播放您的標題，更新統計資料的值使用`set_stat`函式。
1. 這些統計資料更新會定期排清並寫入至 Xbox Live。  您也可以執行此手動。

### <a name="initialization"></a>初始化

您呼叫`StatsManager`初始化所需的資訊與 API 的本機使用者。

如需範例，請參閱下面的

```cpp
std::shared_ptr<stats_manager> statsManager = stats_manager::get_singleton_instance();
statsManager->add_local_user(user);
statsManager->do_work();  // returns stat_event_type::local_user_added
```

```csharp
Microsoft.Xbox.Services.Statistics.Manager.StatisticManager statManager = StatisticManager.SingletonInstance;
statManager.AddLocalUser(user);
statEvent = statManager.DoWork();
```

### <a name="writing-stats"></a>寫入統計資料

您撰寫使用的統計資料`stats_manager::set_stat`系列函式。  有三種變化，此函式，每個資料類型：

* `set_stat_number` 若為浮動。
* `set_stat_integer` 整數。
* `set_stat_string` 字串。

當您呼叫這些動作時，統計資料更新會快取在本機裝置上。  定期這些會被排清至 Xbox Live。

您可以選擇手動排清統計資料，透過`stats_manager::request_flush_to_service`API。  請注意，是否您經常呼叫此函式，您將會受到速率限制。  這不表示永遠不會更新統計資料。  這只是表示逾時到期時，會發生的更新。

```cpp
statsManager->set_stat_integer(user, L"numHeadshots", 20);
statsManager->request_flush_to_service(user); // requests flush to service, performs a do_work
statsManager->do_work();  // applies the stat changes, returns stat_update_complete after flush to service
```

```csharp
statManager.SetStatisticIntegerData(user, statName, (long)statValue);
statManager.RequestFlushToService(user);
statManager.DoWork();
```

#### <a name="example"></a>範例

例如，假設您有第一人稱射擊。  進行比對中，您可能會累積的以下統計資料：

| 狀態名稱 | 格式 |
|-----------|--------|
| 每循最佳的敵人 | 整數 |
| 存留期的敵人 | 整數 |
| 存留期半毀 | 整數 |
| 存留期 Kill/死亡比率 | Number |

因為播放程式會透過比對，您會遞增*刪除每個圓*，*存留期會終止*並*存留期半毀*在本機。

結尾的相符項目會執行下列作業：
1. 比較回合中，使用其先前的最佳中方面有敵人。  如果是更大，然後更新`StatsManager`。
2. 新的值來更新其存留期會終止和半毀，並更新`StatsManager`。
3. 計算敵人/半毀及更新 `StatsManager`

請注意，1 和 2，您需要知道其先前狀態的值。  擷取這些，請參閱上述的各節，如需最佳做法。

任何這些統計資料可能會對應至排行榜，將在下一篇文章中討論。

### <a name="flushing-stats"></a>排清統計資料

您可以手動排清統計資料使用`stats_manager::request_flush_to_service`。  您可能想要這樣做，如果您要顯示排行榜。

例如，如果您有針對排行榜`Lifetime Kills`在上述範例中，您會想要確定，對應到此統計資料的統計資料更新具有已排清到伺服器之前顯示排行榜。  這樣一來排行榜反映玩家的最新的進度。

### <a name="cleanup"></a>Cleanup
當標題關閉時，請從統計資料管理員中移除使用者。 這會將服務，以排清的最新的統計資料值。

```cpp
statsManager->remove_local_user(user);
statsManager->do_work();  // applies the stat changes, returns local_user_removed after flush to service
```

```csharp
statManager.RemoveLocalUser(user);
statManager.DoWork();
```
