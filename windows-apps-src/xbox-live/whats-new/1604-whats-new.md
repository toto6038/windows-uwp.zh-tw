---
title: 新功能-2016 年 4 月 Xbox Live SDK
description: 新功能-2016 年 4 月 Xbox Live SDK
ms.assetid: a6f26ffd-f136-4753-b0cd-92b0da522d93
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9ce63a0174fa0c4158764b8bca2443d58d0aefd9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660763"
---
# <a name="whats-new-for-the-xbox-live-sdk---april-2016"></a>新功能-2016 年 4 月 Xbox Live SDK

請參閱[的新功能-2016 年 3 月](1603-whats-new.md)1603年中新增了哪些功能的文章

## <a name="os-and-tool-support"></a>作業系統和工具支援
Xbox Live SDK 支援 Windows 10 RTM [Version 10.0.10240] 與 Visual Studio 2015 RTM [Version 14.0.23107.0]。

## <a name="tournaments"></a>比賽
- Xbox Live 聯賽工具現在已隨附於 SDK。  您可以看到它的 Tools 目錄，以及如何使用它的某些資訊。
- 比賽的 Api 也會提供。  請參閱 Xbox::Services::Tournaments 命名空間
- 文件也會提供程式設計指南 》 中的。

## <a name="documentation"></a>文件
- [登入疑難排解指南](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md)清單錯誤程式碼以登入失敗，以及步驟進行偵錯的一些一般策略。
- [Marketplace](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-marketplace/marketplace-and-downloadable-content) Xbox One 的開發人員文件只現在可在程式設計指南 》。  UWP 開發人員應該在存放區上的文件的參考合作夥伴中心，以繼續。
- 沒有[XDK UWP 移植指引](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)如果您想要讓通用 Windows 平台 Xbox One 標題，您可以參考到。
- 請參閱[更細緻速率限制](../using-xbox-live/best-practices/fine-grained-rate-limiting.md)文章描述如何這些強制執行各種 Xbox Live 服務端點和案例，以及限制為何的相關資訊。

## <a name="multiplayer-manager"></a>多人遊戲管理員
[多人遊戲 Manager](../multiplayer/multiplayer-manager.md)已不存在於實驗性。  我們已併入使用此 API 的開發人員的意見反應，並做了一些的 Api 彼此更一致。  請使用多人遊戲管理員做為起點時進行多人遊戲開發工作，因為它提供了更簡單的 API，會為您管理許多複雜的多人遊戲 2015 API。

在下列各節，我們列出了一些新功能，API，以及少數的重大變更。

#### <a name="completed-events"></a>已完成的事件
所有 Api 現在會都有對應``` _competed```無論成功或失敗引發事件和所有事件。 先前的行為是，它只觸發發生錯誤時，採取動作標題。 因為呼叫會批次處理，這表示，完成時，標題會取得多個```_competed```事件。

| API | 傳回的事件 |
|-----|----------------|
| ```lobby_session()->set_local_member_properties``` |  ```local_member_property_write_completed ```
| ```lobby_session()->set_local_member_connection_address``` | ```local_member_connection_address_write_completed``` |
| ```lobby_session()->set_properties``` | ```session_property_write_completed``` |
| ```lobby_session()->set_synchronized_properties``` | ```session_synchronized_property_write_completed``` |
| ```lobby_session()->set_synchronized_host``` | ```synchronized_host_write_completed``` |

同樣適用於```game_session()```。

#### <a name="application-defined-context"></a>應用程式定義的內容
您現在可以傳入相互關聯多人遊戲的事件，以起始呼叫每個 set_ * 方法的選擇性應用程式定義內容。
例如

```cpp
_XSAPIIMP xbox_live_result<void> set_properties(
        _In_ const string_t& name,
        _In_ const web::json::value& valueJson,
        _In_ context_t context = nullptr
       );
```

內容會再傳回給透過標題```_completed```事件。

#### <a name="breaking-changes"></a>重大變更

1.  同時邀請 Api (```invite_friends``` & ```invite_users```) 現在已同步。 完成時，它會傳回 invite_sent 事件。

2.  ```write_synchronized_properties_and_commit``` 已重新命名為```set_synchronized_properties```。 完成時，它會傳回```session_synchronized_property_write_completed```事件。

3.  ```write_synchronized_host_and_commit``` 已重新命名為```set_synchronized_host```。 完成時，它會傳回```synchronized_host_write_completed```事件。

4.  在 ```lobby_session()```

  *移除*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
```

  *加入*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

5.  在 ```game_session()```

  *移除*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_arbitration_server& arbitration_server() const;
```
  *加入*

```cpp
_XSAPIIMP const std::unordered_map<string_t, multiplayer_session_reference>& tournament_teams() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

6.  變更事件類型中：

  *移除*

```cpp
write_pending_changes_failed,
tournament_property_changed,
arbitration_property_changed
```

  *重新命名*

  ```write_synchronized_properties_completed``` 重新命名為 ```session_synchronized_property_write_completed```

  ```write_synchronized_host_completed``` 重新命名為 ```synchronized_host_write_completed```
