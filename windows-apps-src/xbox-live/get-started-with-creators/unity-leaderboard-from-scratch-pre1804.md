---
title: 建置在 Unity 的排行榜
description: 建置您自己在 Unity 的排行榜指南
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 unity、 排行榜
ms.openlocfilehash: a62cb2b3bd4afebcc7aa9060db60ac167f052977
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657823"
---
# <a name="leaderboards-data-in-unity"></a>在 Unity 的排行榜資料

對於您想要自訂排行榜經驗，這篇文章會提供您工具來實作您自己的排行榜 Unity 開發人員介紹可用的 Api。 一旦您了解如何提取排行榜資料，您都能夠將它套用到您選擇的使用者介面。

[!IMPORTANT]
> 這篇文章適用於在 2018 年 （1804年發行版本） 5 中所做的更新之前的外掛程式版本。 如果您安裝 Xbox Live 外掛程式之後，或尚未下載您可能會使用不同的呼叫，收集排行榜資料的較新版本。 此外，您會發現，此外掛程式中的螢幕擷取畫面不相符的最新版本。 請改為參閱[從可用的發行項的較新 unity 排行榜](unity-leaderboard-from-scratch.md)。


## <a name="call-for-leaderboard-data"></a>呼叫排行榜資料

若要從您必須呼叫 Xbox Live 的服務要求排行榜資料  `void GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)`

已成功進行這個呼叫，您必須取得`XboxLiveUser`從[登入](unity-prefabs-and-sign-in.md)，有[設定 stat](add-stats-and-leaderboards-in-unity.md)至少一名玩家，和形式的值`LeaderboardQuery`。 如果您沒有已經知道如何登入使用者，或要初始化您排行榜統計資料，您可以讀取連結的文章。 初始化之後統計資料與您 leaderboard 指令碼建立關聯的最簡單方式是加入統計資料 prefabs 的其中一個： `IntegerStat`， `DoubleStat`，或`StringStat`當做公用變數。 您的統計資料需要有它的 ID 屬性在最少為設定這是我們將用來**statName**當我們呼叫排行榜資料的參數。 最後您會需要表單`LeaderboardQuery`物件。
A`LeaderboardQuery`有幾個屬性可以設定會影響傳回的資料：

- **StatName(Required):** 如果未設定未設定為統計資料識別碼值，這您排行榜會擷取資料，統計資料的識別碼，將會傳回任何資料。
- **SocialGroup:** 這個字串的值而定會篩選傳回的排行榜資料為您的朋友、 您最愛的朋友或如果未設定，將會擷取未篩選的全球排行榜。 值""將會傳回未篩選的清單，"all"會傳回與您朋友在其中加入 「 我的最愛 」 清單會傳回與只有您最愛的朋友存在清單的值。
- **SkipResultToRank:** 如果設定，此 uint 變數將會決定項目排名排行榜資料會從開始時傳回。 排名開始排序為 1。
- **SkipResultToMe:** 如果設定為 true，此布林值會導致排行榜資料傳回到開始`XboxLiveUser`用於`GetLeaderboard()`呼叫。
- **順序：** 列舉型別的`Microsoft.Xbox.Services.Leaderboard.SortOrder`有兩個可能的值，遞增和遞減。 將此查詢的變數設定會決定您排行榜的排序次序。 根據預設排行榜會以遞減順序傳回資料。
- **MaxItems:** 此單位決定每次呼叫所傳回的資料列數目上限`GetLeaderboard()`。

形成您 LeaderboardQuery 看起來可能如下所示：

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            StatName = stat.ID,
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

使用此查詢會傳回排名的排行榜從 100 開始的五個資料列各有特定的統計資料。

> [!WARNING]
> 將 SkipResultToRank 高於的內含排行榜的播放程式數目，會導致排行榜来傳回的資料與零個資料列。

既然我們已經擁有所有的部分一起我們可以呼叫`GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)`函式，其中在我們將使用我們帶正負號`XboxLiveUser`(可能`XboxLiveUserInfo.User`) 登入時，作為我們的使用者，從取得和`LeaderboardQuery`我們剛才建立作為我們的查詢。

呼叫函式程式排行榜看起來可能如下所示：

```csharp
using Microsoft.Xbox.Services.Leaderboard;

public void LoadLeaderboard()
    {
        // check to make sure you have a valid stat
        if (this.stat == null)
        {
            // TO DO: Display "Stat not specified" error message!
            return;
        }

        // check to make sure you have an XboxLiveUser
        if (this.xboxLiveUser == null)
        {
            if (XboxLiveUserManager.Instance.UserForSingleUserMode != null
                && XboxLiveUserManager.Instance.UserForSingleUserMode.User != null)
            {
                // set the XboxLiveUser if one is available
                this.xboxLiveUser = XboxLiveUserManager.Instance.UserForSingleUserMode.User;
            }
            else // if you don't have a user signed in display an error message and exit. 
            {
                // TO DO: Display "User Not logged In" error message
                return;
            }
        }

        LeaderboardQuery query;

        query = new LeaderboardQuery
        {
            StatName = this.stat.ID,
            MaxItems = this.maxItems,

        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
    }
```

現在，您可能已經注意到，我們擷取函式的排行榜`GetLeaderboard()`傳回 void，並因此不會傳回我們所需的排行榜資料。 我們實際上會擷取在下一節所述的事件函式的排行榜資料。

## <a name="receive-the-leaderboard-data"></a>接收排行榜資料

若要擷取的排行榜資料，您必須新增接聽的函式，以`StatsManagerComponent`標題的執行個體。 您應該加入下列一行程式碼`Awake()`函式的程式碼： `StatsManagerComponent.Instance.GetLeaderboardCompleted += this.MyGetLeaderboardCompletedFunction`。

```csharp
private void Awake()
    {
        this.EnsureEventSystem();
        XboxLiveServicesSettings.EnsureXboxLiveServicesSettings();

        StatsManagerComponent.Instance.GetLeaderboardCompleted += this.GetLeaderboardCompleted;
    }
```

`StatsManagerComponent`接聽排行榜完成事件。 藉由執行這行程式碼，您將加入到排行榜完成事件發生時要呼叫的函式清單的函式。 在此範例中，呼叫函式`MyGetLeaderBoardCompletedFunction()`，您就可以在自己的指令碼中命名函式。 需要 「 MyGetLeaderboardCompletedFunction 」 採用兩個參數，表示寄件者的物件的函式和`Microsoft.Xbox.Services.Client.StatEventArgs`參數。 您的函式的殼層可能看起來像這樣：

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        //Do Something;
    }
```

此函式應該會的先檢查是否發生錯誤，可以在中找到`StatEventArgs`參數 statArgs。 包含 StatArgs`StatisticEvent`呼叫 EventData 所在`System.Exception`呼叫 ErrorInfo。 您可以發現在擷取排行榜資料時發生錯誤 statArgs.EventData.ErrorInfo 中。 您可以加入簡單的 if 陳述式前面的程式碼，檢查是否有錯誤。

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }
    }
```

確認沒有任何錯誤之後, 將排行榜要求中找到的結果儲存`statArgs.EventData.EventArgs.Result`。 `Result` 是`LeaderBoardResult`物件，其中包含您需要填入您排行榜的資料。 在我們的範例程式碼中，我們會擷取這項資料，並將它傳送至另一個函式，呼叫 LoadResult。

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }

        LeaderboardResultEventArgs leaderboardArgs = (LeaderboardResultEventArgs)statArgs.EventData.EventArgs;
        this.LoadResult(leaderboardArgs.Result);
    }
```

`LeaderboardResult`我們會傳送到 LoadResult 函式的結果後，我們要同時讀取傳回的排行榜資料，以及執行其他的呼叫，以擷取尚未原始呼叫所傳回的排列次序的所有資料。 您會想要將結果儲存在您 leaderboard 指令碼的類別變數就像這樣：

```csharp
using Microsoft.Xbox.Services.Leaderboard;

void LoadResult(LeaderboardResult result)
    {
        this.leaderboardData = result;
    }
```

這很重要，因為`LeaderboardResult`包含 HasNext 屬性，判斷是否有更新版本的一段可以擷取排行榜。 `LeaderboardResult`也會儲存構成排行榜資料列總數的變數。 這些屬性必須瀏覽您排行榜。 提取資料，從您`LeaderBoardResult`只要實作迴圈使用`LeaderboardResults`份`LeaderboardRow`稱為`Rows`。 在我們的範例程式碼只我們將在每個值串連`LeaderboardRow`為字串，以顯示。

```csharp
using Microsoft.Xbox.Services.Leaderboard

void LoadResult(LeaderboardResult result)
{

    this.leaderboardData = result;

    this.leaderBoardText.text = "" // leaderBoardText is a UnityEngine.UI text box.

    foreach (LeaderboardRow row in this.leaderboardData.Rows)
    {
        leaderBoardText.text += string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", row.Rank, row.Gamertag, this.stat.DisplayName, row.Values[0]);
    }
}
```

在我們的範例中，我們使用的順位、 玩家代號、 和值屬性`LeaderBoardResult`填入我們的字串，如排行榜相關聯的統計資料的 DisplayName。

我確定您將能夠進行更多創意與上述所有排行榜資料。

## <a name="navigating-the-leaderboard-data"></a>瀏覽排行榜資料

最常見的執行個體中不會載入每個單一的陣序規範您排行榜，也必須要能夠瀏覽至顯示的使用者排行榜的不同區段的排名。 讓我們假設一百排名玩家有排行榜。 初始呼叫中`GetLeaderboard()`您可以擷取十`LeaderboardRows`並將它們顯示播放器。 播放程式可能想要查看更多比前十個玩家。 取得十位使用者下一組最簡單方式是以判斷是否`LeaderboardResult`最後一次您儲存查詢有多個擷取的資料列，然後呼叫`GetLeaderboard()`使用該 LeaderboardResult`NextQuery`屬性。 下列程式碼會擷取下一步 的排行榜資料集。

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetNextLeaders()
    {
        if(this.leaderboardData.HasNext)
        {
            LeaderboardQuery query = this.leaderboardData.NextQuery;
            XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
        }
        else
        {
            //TO DO: Display an error message or go back to the beginning of the leaderboard as the situation demands.
        }
    }
```

向後移動，在您的排行榜是比較困難，因為沒有任何函式，拉先前的從您的排行榜的排列次序的數目。 若要擷取先前的排名，您必須撰寫您自己的邏輯。 其中一種方法是儲存您`MaxItems`每個`LeaderboardQuery`，並計算您需要使用略過哪些陣序規範`SkipToRank`屬性的程式`LeaderboardQuery`。 該程式碼看起來可能像這樣：

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetPreviousLeader()
{
    if(leaderboardData == null || leaderboardData.Rows.Count < 1)
    {
        return;
    }

    uint topRank = leaderboardData.Rows[0].Rank; //get the first rank of the leaderboard.
    uint targetRank = topRank - this.maxItems > 0 ? topRank - this.maxItems : 0; //set your targetRank equal to the current top rank of your leaderboard minus the configured number of rows to display at a time.

    LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
    {
        StatName = stat.ID,
        SkipResultToRank = targetRank,
        MaxItems = this.maxItems
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
}
```

最後一個最常見的案例是播放程式可能只是想查看其位置在排行榜。 這所呼叫，輕鬆實現`GetLeaderboard()`函式與查詢其中`SkipResultToMe`屬性設為 true。

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

    void GetRankForTag()
    {

        LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
        {
            StatName = stat.ID,
            SkipResultToMe = true,
            MaxItems = this.maxItems
        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
    }
```

如果您想要深入了解的更詳細的排行榜範例一定可以讀取 Leaderboard.cs 中的指令碼 XboxLive 外掛程式資料夾下的資產 >> XboxLive >> 指令碼 >> Leaderboard.cs。