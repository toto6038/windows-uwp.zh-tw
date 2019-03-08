---
title: 處理通訊協定啟用
description: 了解如何使用 Xbox Live 的多人連線管理員來處理通訊協定啟用。
ms.assetid: e514bcb8-4302-4eeb-8c5b-176e23f3929f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 多人連線管理員、 通訊協定啟用
ms.localizationpriority: medium
ms.openlocfilehash: 0b5dead742e18bbf5f3e9c271109352ae48e8fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655833"
---
# <a name="handle-protocol-activation"></a>處理通訊協定啟用

通訊協定啟用時，系統會自動啟動遊戲以回應其他動作，通常當播放程式會接受其他玩家的遊戲的邀請。

您的標題就能透過下列方式啟用的通訊協定：

* 當使用者接受遊戲的邀請
* 當使用者選取 「 加入 Game 」 從玩家的 gamercard。

此案例說明如何處理通訊協定啟動時啟動您的標題，並加入大廳和遊戲，（如果有的話）。

您可以看到這裡的程序流程圖：[流程圖-控點的通訊協定啟動 player](mpm-flowcharts/mpm-on-protocol-activation.md)。

| 方法 | 觸發事件 |
| -----|----------------|
| `multiplayer_manager::join_lobby(IProtocolActivatedEventArgs^ args, User^)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

當播放程式接受遊戲的邀請，或加入朋友的遊戲，玩家的 gamercard 透過時，遊戲將會啟動在其裝置上使用通訊協定啟用。 一次遊戲啟動時，多人連線管理員可以使用的通訊協定啟用的事件引數加入入口。 （選擇性） 如果您沒有加入本機使用者，透過`lobby_session()::add_local_user()`，您可以傳遞清單中的使用者透過`join_lobby()`API。 如果不加入受邀的使用者，或如果邀請另一位使用者已新增使用者`join_lobby()`會失敗，並提供`invited_xbox_user_id()`的一部分傳送的邀請`join_lobby_completed_event_args`。

加入之後大廳，Microsoft 會建議設定成員的本機成員的連線位址，以及任何自訂屬性。 您也可以設定透過主機`set_synchronized_host`如果不存在。

最後，多人遊戲管理員就會自動加入到遊戲工作階段的使用者，如果遊戲已在進行中，而且有空間供受邀者。 標題將會收到通知，透過`join_game_completed`提供適當的錯誤碼和訊息的事件。

**範例：**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args, users);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(),
    connectionAddress);
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
