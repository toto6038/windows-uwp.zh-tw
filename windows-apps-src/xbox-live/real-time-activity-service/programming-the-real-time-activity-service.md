---
title: 程式設計即時活動的服務
description: 深入了解程式設計技術的 Xbox Live 即時活動服務與 c + + Api。
ms.assetid: 98cdcb1f-41d8-43db-98fc-6647755d3b17
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 即時活動
ms.localizationpriority: medium
ms.openlocfilehash: f8846d57343f4f7262bbeea2cec03465fa23b2ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629993"
---
# <a name="programming-the-real-time-activity-service-using-c-apis"></a>程式設計即時活動的服務使用 c + + Api

這篇文章包含下列各節

* 從 Xbox 即時連接到即時活動的服務
* 即時活動的服務中斷連線
* 建立統計資料
* 從即時活動訂閱統計資料
* 取消訂閱從即時活動的服務統計資料
* 範例

## <a name="connecting-to-the-real-time-activity-service-from-xbox-live"></a>從 Xbox 即時連接到即時活動的服務

應用程式必須連接至即時活動 (RTA) 服務，以取得從 Xbox Live 的事件資訊。 本主題說明如何建立這類連接。

> [!NOTE]
> 使用本主題中的範例代表一位使用者的方法呼叫。 不過，標題必須進行這些呼叫來連接及中斷從即時活動 (RTA) 服務的所有使用者。

### <a name="connecting-to-the-real-time-activity-service"></a>連接到即時活動的服務

```cpp
void Example_RealTimeActivity_ConnectAsync()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.

    // Note, only retrieve an XboxLiveContext for a given user *once*.  Otherwise you may encounter unpredictable behavior.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->activate();
}
```

### <a name="creating-a-statistic"></a>建立統計資料

如果您是 XDK 開發人員或工作，XDP 上建立統計資料跨 play 標題。  如果您正在執行的 Windows 10 的純 UWP，您可以建立在合作夥伴中心內的統計資料。

#### <a name="xdk-developers"></a>XDK 的開發人員

如需如何 XDP 上建立統計資料資訊，請參閱[XDP 文件](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_10_27_15_a.aspx#events)。  您已建立您的統計資料，並定義您的事件之後，您必須執行[XCETool](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/atoc_xce_jun15.aspx)來產生應用程式所使用的標頭。  此標頭會包含您可以將修改統計資料的事件傳送至呼叫的函式。

#### <a name="uwp-developers"></a>UWP 開發人員

如果您正在開發 UWP 不是跨 play 標題的 Windows 10 上，您會定義中的程式統計資料[合作夥伴中心](https://partner.microsoft.com/dashboard)。 讀取[合作夥伴中心統計資料組態文章](../leaderboards-and-stats-2017/player-stats-configure-2017.md)以了解如何設定在合作夥伴中心上的統計資料。

> [!NOTE]
> 統計資料 2013年開發人員必須連絡以取得資訊有關其補西牆[Stats 2013 組態](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/windows-configure-stats-2013)中[合作夥伴中心](https://partner.microsoft.com/dashboard)。

### <a name="disconnecting-from-the-real-time-activity-service"></a>中斷服務的即時活動

```cpp
void Example_RealTimeActivity_Disconnect()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->deactivate();
}
```

## <a name="subscribing-to-a-statistic-from-the-real-time-activity"></a>從即時活動訂閱統計資料

應用程式訂閱來即時活動 (RTA) 以取得更新，在 Xbox 開發人員入口網站 (XDP) 」 或 「 合作夥伴中心設定的統計資料變更時。

### <a name="subscribing-to-a-statistic-from-the-real-time-activity-service"></a>從即時活動的服務統計資料訂閱

```cpp
void Example_RealTimeActivity_SubscribeToStatisticChangeAsync()
{
    // Obtain xboxLiveContext as shown in the connect function.  Then add a handler to be called on statistic changes.
    function_context statisticsChangeContext = xboxLiveContext->user_statistics_service().add_statistic_changed_handler(
        [](statistic_change_event_args args )
        {
            // Called on statistic change.  Inspect args to see which one.
            DebugPrint("%S %S", args.latest_statistic().statistic_name().c_str(), args.latest_statistic().value().c_str());
        }
    );

    // Call to subscribe to an individual statistic.  Once the subscription is complete, the handler will be called with the initial value of the statistic.
    auto statisticResults = xboxLiveContext->user_statistics_service().subscribe_to_statistic_change(
        User::Users->GetAt(0)->XboxUserId->Data(),
        L"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",    // Get SCID from "Product Details" page in XDP or the Xbox Live Setup page in Partner Center
         L"YourStat"
        );

    std::shared_ptr<statistic_change_subscription> statisticsChangeSubscription;

    if(!statisticResults.err())
    {
        statisticsChangeSubscription = statisticResults.payload();
    }
    else
    {
        DebugPrint("Error calling SubscribeToStatistic: %S", statisticResults.err_message().c_str());
    }
}
```

## <a name="unsubscribing-from-a-statistic-from-the-real-time-activity-service"></a>取消訂閱從即時活動的服務統計資料

應用程式訂閱統計資料的即時活動 (RTA) 服務，以取得更新統計資料變更時。 當不再需要這些更新時，可以終止訂用帳戶，和本主題中的程式碼示範如何執行該動作。

### <a name="unsubscribing-from-a-real-time-services-statistic"></a>取消訂閱服務的即時統計資料

```cpp
void Example_RealTimeActivity_UnsubscribeFromStatisticChangeAsync()
{
    // statisticsChangeSubscription from the Example_RealTimeActivity_SubscribeToStatisticChangeAsync function.
    xboxLiveContext->user_statistics_service().unsubscribe_from_statistic_change(
        statisticsChangeSubscription
        );
}
```

> [!IMPORTANT]
> 即時活動服務會使用兩個小時之後中斷連線的情況下，您的程式碼必須能夠偵測到此，並重新建立連線至即時活動的服務，但仍需要。 這主要是為了確保到期後會重新整理驗證權杖。
> 
> 如果用戶端對於多人連線的工作階段，會使用 RTA，並中斷連線的 30 秒，多人連線的工作階段 Directory(MPSD) 會偵測到 RTA 工作階段關閉時，一開始會將使用者登出工作階段。 它由 RTA 用戶端偵測關閉連線時，起始重新連線並重新訂閱 MPSD 結束工作階段之前。