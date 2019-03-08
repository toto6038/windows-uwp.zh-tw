---
title: 傳送遊戲邀請
description: 了解如何使用 Xbox Live 的多人連線管理員，讓播放程式傳送遊戲的邀請。
ms.assetid: 8b9a98af-fb78-431b-9a2a-876168e2fd76
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 多人遊戲，遊戲邀請、 流程圖、 多人連線管理員
ms.localizationpriority: medium
ms.openlocfilehash: 85aa45558d1443638ba7dd50dbea8923125ef664
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645993"
---
# <a name="send-game-invites"></a>傳送遊戲邀請

其中一個更簡單的多人遊戲案例可讓玩家玩這個遊戲線上與朋友。 此案例中會涵蓋您需要將邀請傳送給加入您的遊戲的其他播放器的步驟。

之後，您就[初始化多人遊戲管理員](play-multiplayer-with-friends.md)，而且具有[藉由新增本機使用者建立大廳工作階段](play-multiplayer-with-friends.md)，您必須等到您收到`user_added`事件才能開始傳送邀請。

有兩種方式可傳送邀請：

1. [Xbox 平台邀請 TCUI](#xbox-platform-invite-tcui)
2. [標題實作自訂 UI](#title-implemented-custom-ui)

您可以看到這裡的程序流程圖：[流程圖-傳送邀請給其他玩家](mpm-flowcharts/mpm-send-invites.md)。

### <a name="1-xbox-platform-invite-tcui-a-namexbox-platform-invite-tcui"></a>1) Xbox 平台邀請 TCUI <a name="xbox-platform-invite-tcui">

| 方法 | 觸發事件 |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |

呼叫`invite_friends()`邀請朋友標準 Xbox UI 會顯示。 這會顯示 UI，可讓播放程式將選取的朋友或最近邀請至遊戲的玩家。 一旦播放程式叫用確認，多人遊戲管理員就會傳送邀請到選取的播放程式。

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

### <a name="2-title-implemented-custom-uia-nametitle-implemented-custom-ui"></a>標題 2） 實作的自訂 UI<a name="title-implemented-custom-ui">

| 方法 | 觸發事件 |
|-----|----------------|
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

您的標題來檢視線上的朋友及邀請他們可實作自訂 TCUI。 可以使用遊戲`invite_users()`方法以傳送邀請的人員其 Xbox Live 使用者 Id 所定義的一組。 這非常有用，如果您想要使用您自己的遊戲中 UI，而不是股票 Xbox UI。

**範例：**

```cpp
std::vector<string_t>& xboxUserIds;
xboxUserIds.push_back();  // Add xbox_user_ids from your in-game roster list

auto result = mpInstance->lobby_session()->invite_users(xboxUserIds);
if (result.err())
{
  // handle error
}
```

**由多人遊戲管理員執行的函式**

* 直接到選取的播放程式會傳送邀請
