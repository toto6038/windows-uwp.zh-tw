---
title: 排行榜
description: 了解如何使用 Xbox Live 的排行榜比較播放程式。
ms.assetid: 132604f9-6107-4479-9246-f8f497978db7
ms.date: 09/28/2018
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8fd7e30b99418fda614a888d9269548cdc57a88a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662023"
---
# <a name="leaderboards"></a>排行榜

## <a name="introduction"></a>簡介

中所述[的資料平台概觀](../data-platform/data-platform.md)，排行榜是鼓勵您的玩家，之間的競爭，並讓玩家持續參與嘗試打破其先前的最佳分數，以及他們朋友的好方法。

針對排行榜[精選的統計資料](stats2017.md#configured-stats-and-featured-leaderboards)會一律顯示在項目的遊戲中樞和它已釘選到首頁時，有時候顯示的 UI 項目的一部分。 您也可以使用您設定精選的統計資料來建立排行榜內您的標題。

## <a name="choosing-good-leaderboards"></a>選擇良好的排行榜

中所述[Player Stats](player-stats.md)，排行榜對應至您已定義的狀態。  您應該選擇對應至播放程式可以努力改善成就的排行榜。

比方說，賽車遊戲中的最佳單圈時間都是很好的排行榜，因為播放程式會想要努力找出改善其最佳的單圈時間。  其他範例包括戰鬥遊戲中的 Kill/死亡比例射擊，或最大組合大小。

## <a name="when-to-display-leaderboards"></a>當顯示排行榜

您可以隨時在您的標題中顯示排行榜。  您應該選擇當排行榜不會干擾遊戲或標題的流程的時間。  回合之間，以及這兩個很好的時間相符項目之後。

## <a name="how-to-display-leaderboards"></a>如何顯示排行榜

有許多選項可以顯示在 Xbox Live SDK 中提供的排行榜。  如果您使用 Unity 與 Xbox Live 創作者計劃，您可以開始利用排行榜 Prefab 來顯示排行榜資料。  請參閱[Unity 中設定 Xbox Live](../get-started-with-creators/configure-xbox-live-in-unity.md)文件，如需詳細資訊。

如果您直接針對 Xbox Live SDK 編碼，請繼續閱讀以深入了解您可以使用的 Api。

## <a name="programming-guide"></a>程式設計手冊

有數個可用來取得目前狀態的排行榜的排行榜 Api。  所有的 Api 是非同步的並不會封鎖。  您會提出取得排行榜資料，並繼續您平常的遊戲處理要求。  當從服務傳回的排行榜結果時，您可以在適當的時間來顯示結果。

您應該要求排行榜資料從服務中，稍有之前當您想要顯示它，以便播放程式不會封鎖等候要顯示排行榜。

## <a name="leaderboards-2013-apis"></a>排行榜 2013 Api

您可以看到`leaderboard_service`適用於所有的統計資料 2013年排行榜 API 的命名空間。

<table>

<tr>
<td>C++ API</td><td>描述</td>
</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
        const string_t& scid,
        const string_t& name
        );
```

</td>

<td>最基本的 api 版本。  這會傳回給定排行榜，從播放程式頂端的排行榜的排行榜值。</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
        _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName
        ) 
```

</td>

<td>WinRTC#程式碼-取得指定服務的組態識別碼和排行榜名稱的單一排行榜的排行榜。</td>

</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ uint32_t skipToRank,
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>此 API 提供一些更大的彈性，您可以指定您想要顯示此項目，排名 （位置），以及要傳回之項目的最大值。  比方說您會使用此 API，如果您想要顯示的位置開始 1000年排行榜。</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_ uint32 skipToRank,
         _In_ uint32 maxItems
        ) 
```

</td>

<td>WinRTC#程式碼-取得單一排行榜提供服務的組態識別碼和排行榜名稱，結果將會開始於"skipToRank 「 順位的排行榜的排行榜結果頁面。</td>

</tr>

<tr>

<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard_skip_to_xuid(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ const string_t& skipToXuid = string_t(),
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>

如果您想要請排行榜跳至特定使用者，請使用此選項。  A`XUID`是為每個 Xbox 使用者的唯一識別碼。  您可以取得已登入的使用者或任何一種他們朋友，並將它傳遞至這個函式。

</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardWithSkipToUserAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_opt_ Platform::String^ skipToXboxUserId,
         _In_ uint32 maxItems
        )
```

</td>

<td>WinRTC#程式碼-取得開始指定播放程式，不論玩家的排名或依玩家的百分位數排名的分數的排行榜</td>

</tr>

</table>

## <a name="2013-c-example"></a>2013 c + + 範例

使用 c + + API 層時您可以接著設定排行榜結果會傳回從服務叫用的回呼。  我們會顯示在以下的範例。

如果您不熟悉`pplx::task`傳回從這些 Api，這是非同步的工作物件從 Microsoft 平行程式設計程式庫 (PPL)。  您可以深入了解，在[ https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks ](https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks)。

下一節顯示如何擷取排行榜結果和使用它們。

### <a name="1-create-an-async-task-to-retrieve-leaderboard-results"></a>1.建立一項非同步工作以擷取排行榜的結果

第一個步驟是呼叫排行榜服務來擷取特定的排行榜結果。

```cpp
pplx::task<xbox_live_result<leaderboard_result>> asyncTask;
auto& leaderboardService = xboxLiveContext->leaderboard_service();

asyncTask = leaderboardService.get_leaderboard(m_liveResources->GetServiceConfigId(), LeaderboardIdEnemyDefeats);
```

### <a name="2-setup-a-callback"></a>2.安裝程式回撥

您可以設定[接續工作](https://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx#continuations)排行榜結果會傳回之後呼叫。  您這麼做，如下所示如下。

```cpp
asyncTask.then([this](xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result)
{
    // Handle result here
});
```

這個接續工作會呼叫內容中的物件，其原本叫用它，並接收```leaderboard_result```，會顯示哪些方式適合您的標題。


### <a name="3-display-leaderboard"></a>3.顯示排行榜

排行榜資料包含在```leaderboard_result```和欄位是自我說明。  如需範例，請參閱下方內容。

```cpp
auto leaderboard = result.payload();

for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
{
    string_t colValues;
    for (auto columnValue : row.column_values())
    {
        colValues = colValues + L" ";
        colValues = colValues + columnValue;
    }
    m_console->Format(L"%18s %8d %14f %10s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
}

```

## <a name="2013-winrt-c-example"></a>2013 WinRTC#範例

使用 WinRT 時C#層，您不需要進行工作，並將只需要使用不同的回呼`await`關鍵字呼叫排行榜服務時。

### <a name="1-access-the-leaderboardservice"></a>1.存取 LeaderboardService

`LeaderboardService`可以從擷取`XboxLiveContext`建立遊戲的使用者在登入時，您會需要它來呼叫排行榜資料。

```csharp
XboxLiveContext xboxLiveContext = idManager.xboxLiveContext;
LeaderboardService boardService = xboxLiveContext.LeaderboardService;
```

### <a name="2-call-the-leaderboardservice"></a>2.呼叫 LeaderboardService

```csharp
LeaderboardResult boardResult = await boardService.GetLeaderboardAsync(
     xboxLiveConfig.ServiceConfigurationId,
     leaderboardName
     );
```

### <a name="3-retrieve-leaderboard-data"></a>3.擷取排行榜資料

`GetLeaderboardAsync()` 傳回`LeaderboardResult`其中會包含填入已命名的排行榜的統計資料。

`LeaderboardResult` 有數個函式和屬性，以方便讀取排行榜資料。

|屬性  |描述  |
|---------|---------|
|public IAsyncOperation<LeaderboardResult> GetNextAsync(uint maxItems);     |擷取下一組最 maxItems 參數數目的順位。 這是本質上另一個呼叫 `GetLeaderboard()`         |
|public LeaderboardQuery GetNextQuery();     |擷取可用來進行擷取的下一個資料集的排行榜呼叫 LeaderboardQuery。         |
|public bool HasNext { get; }    |指定要擷取的其他排行榜資料列         |
|public IReadOnlyList<LeaderboardRow> Rows { get; }     | 包含每個陣序規範的排行榜資料的資料列        |
|public IReadOnlyList<LeaderboardColumn> Columns { get; }     | 資料行組成的排行榜清單        |
|public uint TotalRowCount { get; }     | 在排行榜中的資料列的總數        |
|public string DisplayName { get; }     | 排行榜顯示名稱       |

將一頁提供排行榜資料一次。 您可能會執行迴圈`LeaderboardResult`來擷取資料的資料列和資料行。  
使用`HasNext`布林值和`GetNextAsync()`函式來擷取排行榜資料更新頁面。

```csharp
if (boardResult != null)
{
    foreach (LeaderboardRow row in boardResult.Rows)
    {
        Debug.Write(string.Format("Rank: {0} | Gamertag: {1} | {2}\n", row.Rank, row.Gamertag, row.Values.ToString()));
    }
}
```

## <a name="leaderboard-2017"></a>排行榜 2017

若要讓您將使用的統計資料 2017年排行榜服務呼叫`StatisticManager`排行榜 api，而不是`LeaderboardService`排行榜 api。  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_leaderboard (
     _In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ leaderboard::leaderboard_query query
     ) 
```  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_social_leaderboard (_In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ const string_t &socialGroup,
     _In_ leaderboard::leaderboard_query query
)
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetLeaderboard`  

```csharp
public void GetLeaderboard(
    XboxLiveUser user,
    string statName,
    LeaderboardQuery query
    )
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetSocialLeaderboard`  

```csharp
public void GetSocialLeaderboard(
    XboxLiveUser user,
    string statName,
    string socialGroup,
    LeaderboardQuery query
    )
```  

## <a name="2017-c-example"></a>2017 c + + 範例

### <a name="1-get-a-singleton-instance-of-the-statsmanager"></a>1.取得 stats_manager 單一執行個體

您可以呼叫之前`stats_manager`函式，您必須將變數設定為它的單一執行個體。

```csharp
m_statsManager = stats_manager::get_singleton_instance();
```

### <a name="2-create-a-leaderboardquery"></a>2.建立 LeaderboardQuery

`leaderboard_query`將指定的數量，依序和排行榜呼叫起始點的資料傳回。

A`leaderboard_query`有幾個屬性可以設定會影響傳回的資料：

|屬性 |描述  |
|---------|---------|
|m_skipResultToRank     |此 uint 變數會決定排名的排行榜資料內容會從開始時傳回。 排名開始排序為 1。         |
|m_skipResultToMe     |如果設定為 true，此布林值會導致排行榜資料傳回到開始`XboxLiveUser`用於`get_leaderboard()`呼叫。  |
|m_order     |列舉型別的`xbox::services::leaderboard::sort_order`有兩個可能的值，遞增和遞減。 將此查詢的變數設定會決定您排行榜的排序次序。        |
|m_maxItems     |此單位決定每次呼叫所傳回的資料列數目上限`get_leaderboard`或`get_social_leaderboard()`。         |

`leaderboard_query` 有數個可用來將值指派給這些屬性的 set 函式。 下列程式碼將示範如何設定程式 `leaderboard_query`

```cpp
leaderboard::leaderboard_query leaderboardQuery;
leaderboardQuery.set_skip_result_to_rank(10);
leaderboardQuery.set_max_items(10);
leaderboardQuery.set_order(sort_order::descending);
```

此查詢會傳回十個資料列，從 100 開始排行榜的排名個別。

> [!WARNING]
> 將 SkipResultToRank 高於的內含排行榜的播放程式數目，會導致排行榜来傳回的資料與零個資料列。

### <a name="3-call-getleaderboard"></a>3.呼叫 get_leaderboard

```cpp
leaderboard::leaderboard_query leaderboardQuery;
m_statsManager->get_leaderboard(user, statName, leaderboardQuery);
```

> [!IMPORTANT]
> `statName`用於`GetLeaderboard()`呼叫必須設定為您的標題中 」 狀態的名稱相同[合作夥伴中心](https://partner.microsoft.com/dashboard)，這是區分大小寫。

### <a name="4-read-the-leaderboard-data"></a>4.讀取排行榜資料

若要讀取的排行榜資料，您必須呼叫`stats_manager::do_work()`函式會傳回一份`stat_event`值。 排行榜資料將會包含在`stat_event`型別的`stat_event_type::get_leaderboard_complete`。 當您遇到這種類型的清單中的事件`stat_event`您可以瀏覽的 s`leaderboard_result`中所包含`stat_event`來存取資料。

範例`do_work()`處理常式

```cpp
void Sample::UpdateStatsManager()
{
    // Process events from the stats manager
    // This should be called each frame update

    auto statsEvents = m_statsManager->do_work();
    std::wstring text;

    for (const auto& evt : statsEvents)
    {
        switch (evt.event_type())
        {
            case stat_event_type::local_user_added: 
                text = L"local_user_added"; 
                break;

            case stat_event_type::local_user_removed: 
                text = L"local_user_removed"; 
                break;

            case stat_event_type::stat_update_complete: 
                text = L"stat_update_complete"; 
                break;

            case stat_event_type::get_leaderboard_complete: //leaderboard data is read here
                text = L"get_leaderboard_complete";
                auto getLeaderboardCompleteArgs = std::dynamic_pointer_cast<leaderboard_result_event_args>(evt.event_args());
                ProcessLeaderboards(evt.local_user(), getLeaderboardCompleteArgs->result());
                break;
        }

        stringstream_t source;
        source << _T("StatsManager event: ");
        source << text;
        source << _T(".");
        m_console->WriteLine(source.str().c_str());
    }
}
```

排行榜資料讀取排行榜結果  

```cpp
void Sample::PrintLeaderboard(const xbox::services::leaderboard::leaderboard_result& leaderboard)
{
    if (leaderboard.rows().size() > 0)
    {
        m_console->Format(L"%16s %6s %12s %12s\n", L"Gamertag", L"Rank", L"Percentile", L"Values");
    }

    for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
    {
        string_t colValues;
        for (auto columnValue : row.column_values())
        {
            colValues = colValues + L" ";
            colValues = colValues + columnValue;
        }
        m_console->Format(L"%16s %6d %12f %12s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
    }
}
```  

從排行榜擷取進一步的資料頁。  

```cpp
void Sample::ProcessLeaderboards(
    _In_ std::shared_ptr<xbox::services::system::xbox_live_user> user,
    _In_ xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result
    )
{
    if (!result.err())
    {
        auto leaderboardResult = result.payload();
        PrintLeaderboard(leaderboardResult);

        // Keep processing if there is more data in the leaderboard
        if (leaderboardResult.has_next())
        {
            if (!leaderboardResult.get_next_query().err())
            {               
                auto lbQuery = leaderboardResult.get_next_query().payload();
                if (lbQuery.social_group().empty())
                {
                    m_statsManager->get_leaderboard(user, lbQuery.stat_name(), lbQuery);
                }
                else
                {
                    m_statsManager->get_social_leaderboard(user, lbQuery.stat_name(), lbQuery.social_group(), lbQuery);
                }
            }
        }
    }
}
```  

## <a name="2017-winrt-c-example"></a>2017 WinRTC#範例

### <a name="1-get-a-singleton-instance-of-the-statisticmanager"></a>1.取得 StatisticManager 的單一執行個體

您可以呼叫之前`StatisticManager`函式，您必須將變數設定為它的單一執行個體。

```csharp
statManager = StatisticManager.SingletonInstance;
```

### <a name="2-create-a-leaderboardquery"></a>2.建立 LeaderboardQuery

`LeaderboardQuery`將指定的數量，依序和排行榜呼叫起始點的資料傳回。  

```csharp
public sealed class LeaderboardQuery : __ILeaderboardQueryPublicNonVirtuals
    {
        [Overload("CreateInstance1")]
        public LeaderboardQuery();

        public bool HasNext { get; }
        public string SocialGroup { get; }
        public string StatName { get; }
        public SortOrder Order { get; set; }
        public uint MaxItems { get; set; }
        public uint SkipResultToRank { get; set; }
        public bool SkipResultToMe { get; set; }
    }
```

A`LeaderboardQuery`有幾個屬性可以設定會影響傳回的資料：

|屬性 |描述  |
|---------|---------|
|SkipResultToRank     |此 uint 變數會決定排名的排行榜資料內容會從開始時傳回。 排名開始排序為 1。         |
|SkipResultToMe     |如果設定為 true，此布林值會導致排行榜資料傳回到開始`XboxLiveUser`用於`GetLeaderboard()`呼叫。  |
|順序     |列舉型別的`Microsoft.Xbox.Services.Leaderboard.SortOrder`有兩個可能的值，遞增和遞減。 將此查詢的變數設定會決定您排行榜的排序次序。        |
|MaxItems     |此單位決定每次呼叫所傳回的資料列數目上限`GetLeaderboard()`或`GetSocialLeaderboard()`。         |

形成您`LeaderboardQuery`看起來可能如下所示：

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

此查詢會傳回排名的排行榜從 100 開始的五個資料列個別。

> [!WARNING]
> 將 SkipResultToRank 高於的內含排行榜的播放程式數目，會導致排行榜来傳回的資料與零個資料列。

### <a name="3-call-getleaderboard"></a>3.呼叫 GetLeaderboard()

您現在可以呼叫`GetLeaderboard()`與您`XboxLiveUser`，您的統計資料的名稱和`LeaderboardQuery`。

```csharp
statManager.GetLeaderboard(xboxLiveUser, statName, leaderboardQuery);
```

> [!IMPORTANT]
> `statName`用於`GetLeaderboard()`呼叫必須設定為您的標題中 」 狀態的名稱相同[合作夥伴中心](https://partner.microsoft.com/dashboard)，這是區分大小寫。

### <a name="4-read-leaderboard-data"></a>4.讀取排行榜資料

若要讀取的排行榜資料，您必須呼叫`StatisticManager.DoWork()`函式會傳回一份`StatisticEvent`值。 排行榜資料將會包含在`StatisticEvent`型別的`GetLeaderboardComplete`。 當您遇到這種類型的清單中的事件`StatisticEvent`您可以瀏覽的 s`LeaderboardResult`中所包含`StatisticEvent`來存取資料。

```csharp
IReadOnlyList<StatisticEvent> statEvents = statManager.DoWork(); //In practice this should be called every update frame

foreach(StatisticEvent statEvent in statEvents)
{
    if(statEvent.EventType == StatisticEventType.GetLeaderboardComplete
        && statEvent.ErrorCode == 0)
    {
        LeaderboardResultEventArgs leaderArgs = (LeaderboardResultEventArgs)statEvent.EventArgs;
        LeaderboardResult leaderboardResult = leaderArgs.Result;
        foreach(LeaderboardRow leaderRow in leaderboardResult.Rows)
        {
            Debug.WriteLine(string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", leaderRow.Rank, leaderRow.Gamertag, "test", leaderRow.Values[0]));
        }
    }
}
```

在程式碼`StatisticManager.DoWork()`應該用來處理所有的統計資料管理員內送事件，而不只是如排行榜。 

> [!NOTE]
> 若要擷取`LeaderboardResultEventArgs`您將必須轉型`StatisticEvent.EventArgs`做為`LeaderboardResultEventArgs`變數。

### <a name="5-retrieve-more-leaderboard-data"></a>5.擷取更多的排行榜資料

若要擷取您要使用的排行榜資料更新頁面`LeaderboardResult.HasNext`屬性和`LeaderboardResult.GetNextQuery()`函式來擷取`LeaderboardQuery`這會顯示在下的一個資料頁面。

```csharp
while (leaderboardResult.HasNext)
{
    leaderboardQuery = leaderboardResult.GetNextQuery();
    statManager.GetLeaderboard(xboxLiveUser, leaderboardQuery.StatName, leaderboardQuery)
    // StatisticManager.DoWork() is called
    // Leaderboard results are read
}
```