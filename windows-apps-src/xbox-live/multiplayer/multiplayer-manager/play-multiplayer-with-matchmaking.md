---
title: 使用 SmartMatch 玩多人連線遊戲
description: 了解如何使用 Xbox Live 的多人連線管理員，讓播放程式使用 SmartMatch 尋找多人連線遊戲。
ms.assetid: f9001364-214f-4ba0-a0a6-0f3be0b2f523
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 多人遊戲，多人連線管理員、 流程圖、 smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 0c0ba897f23eb690c3044b00cdb4ce3bea975e71
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655953"
---
# <a name="find-a-multiplayer-game-by-using-smartmatch"></a>使用 SmartMatch 尋找多人連線遊戲

有時候，玩家可能沒有足夠的朋友線上他們想要玩遊戲時，或他們想要針對隨機探尋線上玩時。 您可以使用 Xbox Live SmartMatch 服務來尋找其他 Xbox Live 的玩家

本頁涵蓋您需要使用多人遊戲管理員實作 SmartMatch 配對的基本步驟。

## <a name="find-a-match"></a>尋找相符項目

有四個步驟涉及使用傳送邀請給使用者的好友，然後再針對該位朋友在遊戲加入進行中的多人遊戲 Manager 時：

1. [初始化多人遊戲管理員](#initialize-multiplayer-manager)
2. [藉由新增本機使用者建立大廳工作階段](#create-lobby)
3. [將邀請傳送給朋友](#send-invites)
4. [接受邀請](#accept-invites)
5. [尋找相符項目](#find-match)

步驟 1、 2、 3 和 5 是對裝置執行的邀請。  步驟 4 會通常是透過通訊協定啟動下列應用程式啟動的受邀者的電腦上所起始。

您可以看到這裡的程序流程圖：[流程圖-Play 多人遊戲使用 SmartMatch 配對](mpm-flowcharts/mpm-play-with-smartmatch-matchmaking.md)。

### <a name="1-initialize-multiplayer-manager-a-nameinitialize-multiplayer-manager"></a>1） 初始化多人遊戲管理員 <a name="initialize-multiplayer-manager">

| 會議 | 觸發事件 |
|-----|----------------|
| `multiplayer_manager::initialize(lobbySessionTemplateName)` | 無 |

大廳工作階段物件會自動建立時初始化多人遊戲管理員 中，假設，為指定有效的工作階段範本名稱 （在服務組態中設定）。 請注意這不會在服務上建立大廳工作階段執行個體。

**範例：**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

mpInstance->initialize(lobbySessionTemplateName);
```


### <a name="2-create-the-lobby-session-by-adding-local-usersa-namecreate-lobby"></a>2） 建立大廳工作階段藉由新增本機使用者<a name="create-lobby">

| 方法 | 觸發事件 |
|-----|----------------|
| `multiplayer_lobby_session::add_local_user()` | `user_added_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

在這裡，您將新增本機簽署在 Xbox Live 使用者大廳工作階段中。 第一個使用者時，這會裝載新的入口。 對於所有其他使用者，它們會與次要使用者新增至現有的入口。 此 API 也會通告大廳在 shell 中的朋友分享。 您可以傳送邀請，設定大廳屬性，加入本機使用者之後，只存取透過 lobby() 大廳成員。

加入之後大廳，Microsoft 會建議設定成員的本機成員的連線位址，以及任何自訂屬性。

您必須為所有本機登入使用者重複此程序。

**範例: （單一本機使用者）**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

auto result = mpInstance->lobby_session()->add_local_user(xboxLiveContext->user());
if (result.err())
{
  // handle error
}

// Set member connection address
string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);

// Set custom member properties
mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
```

**範例: （多個本機使用者）**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();
string_t connectionAddress = L"1.1.1.1";

for (User^ user : User::Users)
{
  if( user->IsSignedIn )
  {
    auto result = mpInstance.lobby_session()->add_local_user(user);
    if (result.err())
    {
      // handle error
    }

    // Set member connection address
    mpInstance->lobby_session()->set_local_member_connection_address(
        xboxLiveContext->user(), connectionAddress);

    // Set custom member properties
    mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
  }
}

```


所做的變更會在下一次批次處理`do_work()`呼叫。  
多人遊戲 manager 引發`user_added`事件每次將使用者新增到大廳的工作階段。 建議您檢查以查看該使用者是否已成功加入事件的錯誤碼。 萬一發生失敗，錯誤訊息會提供詳述失敗的原因。

**由多人遊戲管理員執行的函式**

* 註冊時進行即時活動和多人遊戲與 Xbox Live 的多人遊戲服務的訂用帳戶
* 建立大廳工作階段
* 加入為作用中的所有本機播放程式
* 上傳 SDA
* 設定成員屬性
* 註冊以進行工作階段變更事件
* 設定大廳的工作階段為作用中的工作階段

### <a name="3-send-invites-to-friends-optional-a-namesend-invites"></a>3） 傳送邀請給朋友 （選擇性） <a name="send-invites">

| 方法 | 觸發事件 |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

接下來，您會想邀請朋友的標準 Xbox ui。 這會顯示 UI，可讓播放程式將選取的朋友或最近邀請至遊戲的玩家。 一旦播放程式叫用確認，多人遊戲管理員就會傳送邀請到選取的播放程式。

也可以使用遊戲`invite_users()`方法以傳送邀請的人員其 Xbox Live 使用者 Id 所定義的一組。 這非常有用，如果您想要使用您自己的遊戲中 UI，而不是股票 Xbox UI。

**範例：**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**由多人遊戲管理員執行的函式**

* 將註冊 Xbox 股票標題可呼叫 UI (TCUI)
* 直接到選取的播放程式會傳送邀請

### <a name="4-accept-invites-optional-a-nameaccept-invites"></a>4） 接受邀請 （選擇性） <a name="accept-invites">

| 方法 | 觸發事件 |
| -----|----------------|
| `multiplayer_manager::join_lobby(Windows::ApplicationModel::Activation::IProtocolActivatedEventArgs^ args)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

當受邀的播放程式接受遊戲的邀請，或加入朋友遊戲，透過 UI 的殼層時，啟動遊戲在其裝置上使用通訊協定啟用。 一次遊戲啟動時，多人連線管理員可以使用的通訊協定啟用的事件引數加入入口。 （選擇性） 如果您尚未新增 lobby_session()::add_local_user() 透過本機使用者，您可以在清單中的使用者透過傳遞 join_lobby() API。 如果受邀的使用者不會加入透過 lobby_session()::add_local_user()，或是透過 join_lobby()，則 join_lobby() 會失敗，並提供 invited_xbox_user_id() join_lobby_completed_event_args() 的一部分傳送的邀請。

加入之後大廳，Microsoft 會建議設定成員的本機成員的連線位址，以及任何自訂屬性。 如果不存在，您也可以設定透過 set_synchronized_host 主應用程式。

最後，多人遊戲管理員就會自動加入到遊戲工作階段的使用者，如果遊戲已在進行中，而且有空間供受邀者。 標題將會透過提供適當的錯誤碼和訊息 join_game_completed 事件通知。

**範例：**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);

```

錯誤/成功處理透過`join_lobby_completed`事件

**由多人遊戲管理員執行的函式**

* 註冊 RTA & 多人遊戲的訂用帳戶
* 加入大廳工作階段
 * 現有的大廳狀態清除
 * 加入為作用中的所有本機播放程式
 * 上傳 SDA
 * 設定成員屬性
* 註冊以進行工作階段變更事件
* 設定大廳的工作階段為作用中的工作階段
* 加入遊戲工作階段 (如果存在)
 * 使用傳輸控制代碼


### <a name="5-find-match-a-namefind-match"></a>5） 尋找相符項目 <a name="find-match">

| 會議 | 觸發事件 |
|-----|----------------|
| `multiplayer_manager::find_match()` | 許多事件，如中所述```match_status```(例如： ```searching```， ```found```，```measuring```等等) |

之後邀請，如果已接受的話，且主機已準備好開始玩遊戲時，您可以使用 SmartMatch 至現有的遊戲具有足夠開啟的播放程式位置的所有成員大廳工作階段中藉由呼叫其中一個尋找`find_match()`，或建立新的遊戲 se與他人尋求相同的遊戲類型的相符項目，藉由呼叫包含的所有開啟的點組成的填滿大廳工作階段與成員的工作`join_game_from_lobby()`後面接著`set_auto_fill_members_during_matchmaking()`。

您可以呼叫之前`find_match()`，您必須先在您的服務組態中設定 hoppers。 Hopper 定義 SmartMatch 用來比對玩家的規則。

**範例：**

```cpp
auto result = mpInstance.find_match(HOPPER_NAME);
if (result.err())
{
  // handle error
}
```

**由多人遊戲管理員執行的函式**

* 建立比對票證
* 處理所有的 QoS 階段
* 處理名冊變更
 * 重新提交 （如有需要）
* 聯結目標遊戲工作階段
* 通告 via 大廳工作階段的遊戲
