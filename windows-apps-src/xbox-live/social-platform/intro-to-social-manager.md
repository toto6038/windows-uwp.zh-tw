---
title: 身分證 Manager 簡介
description: 深入了解 Xbox Live 社交管理員 API 來追蹤線上朋友。
ms.assetid: d4c6d5aa-e18c-4d59-91f8-63077116eda3
ms.date: 03/26/2018
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5dff3dcfd79fe43ff8af1513a4358bd0ff98b8d1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609003"
---
# <a name="introduction-to-social-manager"></a>身分證 Manager 簡介

## <a name="description"></a>描述

Xbox Live 提供豐富的社交圖形項目可用於各種案例。

使用社交 Api 取得並維護社交圖形的相關資訊在 Xbox Live API (XSAPI) 很複雜，而且讓這項資訊保持在最新狀態可能會很複雜。  未正確地這會導致效能問題、 過時的資料，或超過所需更頻繁地呼叫 Xbox Live 社會服務，而遭到節流。

身分證 Manager 可以解決這個問題：

* 建立簡單的 API 呼叫
* 建立使用即時活動的服務在幕後的最新狀態資訊
* 開發人員可以呼叫同步而不需要任何額外的負擔，在服務上的身分證 Manager API

身分證 Manager 處理多個 RTA 訂用帳戶和使用者的重新整理資料的複雜度消失，並可讓開發人員輕鬆地取得最新狀態的圖形，他們想要建立有趣的案例。

以查看社交 Manager 記憶體和效能特性會看看[社交 Manager 記憶體和效能概觀](social-manager-memory-and-performance-overview.md)

## <a name="features"></a>功能

身分證 Manager 提供下列功能：

* 簡化的社交 API
* 最新的社交圖形
* 控制顯示資訊的詳細資訊
* 降低對 Xbox Live 服務的呼叫數
  * 這就直接在資料擷取的整體延遲降低相互關聯
* 具備執行緒安全
* 有效率地讓資料保持最新狀態

## <a name="core-concepts"></a>核心概念

**社交圖形**:A*社交圖形*建立為在裝置上的本機使用者。 這會建立保留的所有使用者朋友的相關資訊的最新的結構。

> [!NOTE]
> 在 Windows 上只能有一個本機使用者

**Xbox 身分證使用者**:*Xbox 共享使用者*是一組完整的使用者從群組相關聯的社交資料

**Xbox 身分證的使用者群組**:群組是使用者的集合，用來填入 UI 等項目。 有兩種群組類型

* **篩選群組**:篩選群組需要本機 （呼叫） 使用者*社交圖形*並傳回一致全新的使用者根據指定的篩選條件的參數集
* **使用者群組**:使用者群組是使用者的清單，並傳回這些使用者以一致的方式重新檢視。 這些使用者可以是外部使用者的好友名單。

為了避免*社交使用者群組*日期函式的最多`social_manager::do_work()`必須呼叫每個畫面。

## <a name="api-overview"></a>API 概觀

您將最常使用的下列主要類別：

### <a name="social-manager"></a>身分證管理員

* C + + API 類別名稱： social_manager
* WinRT (C#) API 的類別名稱：[SocialManager](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.socialmanager?view=xboxlive-dotnet-2017.11.20171204.01)

這是單一類別，可用來取得**Xbox 共享的使用者群組**這檢視上面所述。

身分證管理員會保留 xbox 共享的使用者群組最新狀態，而且可以篩選由使用者的關聯性存在的使用者群組。  例如，您無法建立 xbox 共享的使用者群組包含所有使用者的好友都在線上，以及玩目前標題。  這會保留最新朋友啟動或停止播放標題。

### <a name="xbox-social-user-group"></a>Xbox 共享的使用者群組

* C + + API 類別名稱： xbox_social_user_group
* WinRT (C#) API 的類別名稱：[XboxSocialUserGroup](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.xboxsocialusergroup?view=xboxlive-dotnet-2017.11.20171204.01)

符合特定準則時，如上面所述的使用者群組。 Xbox 共享的使用者群組公開 （expose） 何種追蹤哪些使用者群組，或設定的篩選條件，以及本機使用者群組所屬。

您可以找到完整說明中的身分證 Manager api [Xbox Live API 參考](https://aka.ms/xboxliveuwpdocs)。
您也可以找到中的 WinRT Api [Microsoft.Xbox.Services.Social.Manager.Namespace 文件](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager?view=xboxlive-dotnet-2017.11.20171204.01)

## <a name="usage"></a>用途

### <a name="creating-a-social-user-group-from-filters"></a>從 篩選器建立社交使用者群組

在此案例中，您會想從篩選條件，例如這位使用者正在關注或標記為我的最愛的所有人的使用者清單。

#### <a name="source-example-using-the-c-api"></a>使用 c + + API 的來源範例

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>來源範例使用C#API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}

```

#### <a name="events-returned"></a>傳回事件

`local_user_added`（C + +） |`LocalUserAdded`(C#)-使用者的社交圖形的載入完成時，觸發程序。 會指出是否在初始化期間發生任何錯誤

`social_user_group_loaded`（C + +） |`SocialUserGroupLoaded`(C#)-在建立社交使用者群組時，觸發程序

`users_added_to_social_graph`（C + +） |`UsersAddedToSocialGraph`(C#)-使用者會在載入時觸發

#### <a name="additional-details"></a>其他詳細資料

上述範例顯示如何初始化使用者的身分證管理員、 建立社交的使用者群組，該使用者，以及保持在最新狀態。

篩選選項中所見`presence_filter`和`relationship_filter`列舉

在遊戲迴圈中，`do_work`函式會更新所有已建立的檢視表的最新的快照集，該群組中的使用者。

在檢視中的使用者，可由呼叫`xbox_social_user_group::get_users()`函式會傳回一份`xbox_social_user`物件。  `xbox_social_user`包含社交的資訊，例如玩家代號、 gamerpic uri 等。

### <a name="create-and-update-a-social-user-group-from-list"></a>建立和更新清單的社交使用者群組

在此案例中，您的使用者，例如使用者清單的多人連線的工作階段中的社交資訊。

#### <a name="source-example-using-the-c-api"></a>使用 c + + API 的來源範例

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_list(
    xboxLiveContext->user(),
    userList  // this is a std::vector<string_t> (list of xuids)
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>來源範例使用C#API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromList(
     xboxLiveContext.User,
     userList //this is a IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
    );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>傳回事件

`local_user_added`（C + +） |`LocalUserAdded`(C#)-使用者的社交圖形的載入完成時，觸發程序。 會指出是否在初始化期間發生任何錯誤

`social_user_group_loaded`（C + +） |`SocialUserGroupLoaded`(C#)-在建立社交使用者群組時，觸發程序

`users_added_to_social_graph`（C + +） |`UsersAddedToSocialGraph`(C#)-使用者會在載入時觸發

### <a name="updating-social-user-group-from-list"></a>更新清單中的身分證使用者群組

您也可以藉由呼叫 update_social_user_group() 變更追蹤中的使用者清單的社交使用者群組

#### <a name="source-example-using-the-c-api"></a>使用 c + + API 的來源範例

```cpp
//#include "Social.h"

socialManager->update_social_user_group(
    xboxSocialUserGroup,
    newUserList    // std::vector<string_t> (list of xuids)
    );

    while(true)
    {
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
    }
```

#### <a name="source-example-using-the-c-api"></a>來源範例使用C#API

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.UpdateSocialUserGroup(
     xboxSocialUserGroup,
     newUserList //IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>傳回事件

`social_user_group_updated`（C + +） |`SocialUserGroupUpdated`(C#)-社交使用者群組更新已完成時，會觸發。

`users_added_to_social_graph` | `UsersAddedToSocialGraph`(C#)-使用者會在載入時，會觸發。 如果使用者透過清單新增已經在圖形中，不會觸發此事件

### <a name="using-social-manager-events"></a>使用社交管理員事件

身分證 Manager 還會告訴您事件的形式中發生的事件。  若要更新您的 UI，或執行其他的邏輯，您可以使用這些事件。

#### <a name="source-example-using-the-c-api"></a>使用 c + + API 的來源範例

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();
socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::no_extra_detail
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

socialManager->do_work();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    auto events = socialManager->do_work();
    for(auto evt : events)
    {
        auto affectedUsersFromGraph = socialUserGroup->get_users_from_xbox_user_ids(evt.users_affected());
        // TODO: render the changes to the friends list using game UI, passing in affectedUsersFromGraph
    }
}
```

##### <a name="source-example-using-the-c-api"></a>來源範例使用C#API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

socialManager.DoWork();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    IReadOnlyList<SocialEvent> Events = socialManager.DoWork();
    IReadOnlyList<XboxSocialUser> affectedUsersFromGraph;
    foreach(SocialEvent managerEvent in Events)
    {
        affectedUsersFromGraph = socialUserGroup.GetUsersFromXboxUserIds(managerEvent.UsersAffected);
    }
}

```

#### <a name="events-returned"></a>傳回事件

`local_user_added`（C + +） |`LocalUserAdded`(C#)-使用者的社交圖形的載入完成時，觸發程序。 會指出是否在初始化期間發生任何錯誤

`social_user_group_loaded`（C + +） |`SocialUserGroupLoaded`(C#)-在建立社交使用者群組時，觸發程序

`users_added_to_social_graph`（C + +） |`UsersAddedToSocialGraph`(C#)-使用者會在載入時觸發

#### <a name="additional-details"></a>其他詳細資料

這個範例會示範部分提供額外的控制。  而不是依賴社交的使用者群組篩選器，在遊戲迴圈期間提供全新的使用者清單，社交圖形被初始化遊戲迴圈外。  則標題會依賴`events`所傳回`socialManager->do_work()`函式。  `events` 是一份`social_event`，而且每個`social_event`包含社交圖形最後一個畫面格期間所發生的變更。  比方說`profiles_changed`，`users_added`等等。詳細的資訊可在`social_event`API 文件。

### <a name="cleanup"></a>Cleanup

#### <a name="cleaning-up-social-user-groups"></a>清除 社交使用者群組

```cpp
//#include "Social.h"

socialManger->destroy_social_user_group(
    socialUserGroup
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.DestroySocialUserGroup(
     socialUserGroup
     );
```

清除所建立的社交的使用者群組。 呼叫端應該也會移除任何參照，因為它現在包含過時的資料有任何建立的社交使用者群組。

#### <a name="cleaning-up-local-users"></a>清除 本機使用者

```cpp
//#include "Social.h"

socialManger->remove_local_user(
    xboxLiveContext->user()
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.RemoveLocalUser(
     xboxLiveContext.User
     );
```

已載入的使用者的社交圖形，以及使用該使用者所建立的任何社交使用者群組，請移除本機使用者移除。

#### <a name="events-returned"></a>傳回事件

`local_user_removed`（C + +） |`LocalUserRemoved`(C#)-已成功移除本機使用者時觸發