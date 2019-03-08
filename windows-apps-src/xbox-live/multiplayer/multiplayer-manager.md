---
title: 多人遊戲管理員
description: 深入了解 Xbox Live 多人連線管理員 中，旨在讓您更輕鬆地實作多人遊戲的高層級 API。
ms.assetid: f3a6c8bc-4f73-4b99-ac51-aadee73c8cfa
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 99159b5d126c671b07d37e20f1bcd61452c7d670
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594713"
---
# <a name="multiplayer-manager"></a>多人遊戲管理員

Xbox Live 提供廣泛的支援將多人遊戲的功能新增至您的項目，可讓您的遊戲世界各地連接 Xbox Live 的成員。  這包括豐富的配對案例中，加入朋友的遊戲進度和其他播放程式的能力。 不過，實作利用多人遊戲 2015 Api 直接的 Xbox Live 多人遊戲是設計的複雜的工作，需要大程度和測試，以確認您遵循的最佳作法，並符合認證需求。

多人遊戲 Manager 可輕鬆將多人遊戲的功能加入您的遊戲，藉由管理工作階段和配對，並提供狀態，並以事件為基礎的程式設計模型。 它是一組 Api 可讓您更容易實作多人遊戲情節，您的 Xbox Live 的遊戲。 它提供導向解決常見的多人遊戲案例，例如玩多人連線遊戲與朋友的 API，邀請處理遊戲，進度、 配對，等等中的處理聯結。 它支援多個本機使用者，並讓您的標題，將多人遊戲工作階段目錄，如果您使用第三方配對服務更容易。 許多這些案例可以完成只需要幾個 API 呼叫。

## <a name="key-features"></a>重要功能
多人遊戲管理員 API 的主要功能如下：

* 簡單的工作階段管理及 Xbox Live 配對
* 狀態和事件式程式設計模型。
* 可確保 Xbox Live 的最佳作法，以及多人遊戲 XR 相容。
* 支援 Xbox One XDK 及 UWP 的標題。
* Implements[多人遊戲 2015年流程圖](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Xbox%20One%20Multiplayer%202015%20Developer%20Flowcharts.aspx)。
* 與傳統的多人遊戲 2015 Api 一起運作。

>**重要**-您的遊戲仍必須實作才能傳遞認證的必要的事件線上多人遊戲。

## <a name="overview"></a>概觀
多人遊戲管理員會導向幾個重要概念：
* `lobby_session` :持續性的工作階段，用來管理這個裝置，您受邀的朋友想要一起播放的本機使用者。 群組可能扮演多個回合、 對應、 層級等進行的遊戲，並大廳工作階段追蹤此核心群好友 （包括本機裝置的播放程式）。 一般而言，主應用程式可能會瀏覽功能表中，透過群組成員，決定他們想要玩哪些遊戲模式時，會形成這個群組。

* `game_session` :追蹤玩家玩遊戲的特定執行個體。 例如，競爭、 地圖或層級。 您可以建立新的遊戲工作階段，透過`join_game_from_lobby`大廳工作階段中包含的成員。  當成員接受邀請時，它們會加入大廳和在遊戲工作階段 （如果有足夠的空間）。 其他播放程式可能會加入遊戲工作階段，如果已啟用配對，但這些額外的播放程式不會新增至大廳工作階段。 這表示，遊戲結束之後, 大廳工作階段中的播放程式仍在一起，而不是從配對的額外播放程式。

* `multiplayer_member` :代表可登入本機或遠端裝置上的個別使用者。

* `do_work` :可確保適當的遊戲狀態更新會維護標題和 Xbox Live 多人遊戲服務之間。 若要確保最佳效能，do_work() 必須呼叫常見問題，例如一次，每個畫面。 它提供您一份`multiplayer_event`回呼事件來處理遊戲。

## <a name="state-machine"></a>狀態機器
`do_work()`呼叫是為了確保您的狀態會保留最新。  針對多人遊戲 Manager 來執行其工作，開發人員必須先呼叫`do_work()`定期方法。 若要這樣做的最可靠方式就是至少一次呼叫每個畫面。 `do_work()` 當它有沒有工作来執行，這樣就不需要顧慮往往呼叫它時，請傳回快速。

多人遊戲管理員 API 所傳回的所有物件不應視為安全執行緒。 不過，它可讓您得以進行執行緒同步處理，如果您將會從多個執行緒呼叫它。 媒體櫃有內部的多執行緒保護，但您仍必須實作您自己鎖定，如果您需要一個執行緒存取的任何值 – 比方說，查核 members() 清單，而另一個執行緒可能會叫用`do_work()`。

多人遊戲管理員會維護當玩家加入、 離開，或已更新工作階段時，更新在背景中的工作階段狀態基礎模型。 為了協助避免您遊戲的執行緒與 UI 執行緒的執行緒同步處理問題，多人遊戲管理員不會更新為應用程式的可見狀態的工作階段，直到您呼叫`do_work()`方法。 傳統上，您會接收事件，例如在背景執行緒上的工作階段變更的相關通知，並再也不必與 UI 執行緒，以顯示這些變更同步處理。 使用 [多人遊戲管理員] 中，這項工作會為您完成在背景。  您可以呼叫`do_work()`在主執行緒在您選擇的時間，以取得最新狀態的快照集的多人遊戲 Manager 正在緩衝，在幕後。

## <a name="events-and-notifications"></a>事件和通知
多人遊戲管理員會定義一組大量的事件 (請參閱`multiplayer_event_type`)，並通知透過標題`do_work()`所發生的任何方法。 例如加入或離開，成員屬性變更，遠端的播放程式或工作階段狀態變更事件。 所有的多人遊戲管理員 Api 均為同步。 `do_work()`方法會傳回一份這些非同步作業完成的事件。 視情況需要，您的標題應該處理這些事件。 請參閱`multiplayer_event`類別文件，如需詳細資訊。

針對每個事件，根據事件類型，您必須轉換`event_args`至適當的事件引數類別，以取得事件屬性。 下列範例示範如何使用`do_work()`來處理事件：

```cpp
auto eventQueue = mpInstance.do_work();
for (auto& event : eventQueue)
{
    switch (event.event_type())
    {
      case multiplayer_event_type::member_joined:
      {
        auto memberJoinedArgs =  std::dynamic_pointer_cast<member_joined_event_args>(event.event_args());
        HandleMemberJoined(memberJoinedArgs);
        break;
      }
      case multiplayer_event_type::session_property_changed:
      {
        auto sessionPropertyChangedArgs =  std::dynamic_pointer_cast<session_property_changed_event_args>(event.event_args());
        HandlePropertiesChanged(sessionPropertyChangedArgs);
        break;
      }
      . . .
    }
}

```

## <a name="scenarios"></a>案例

這一節我們將探討一些常見的案例和每個案例中，您可以呼叫的 Api。  此外，也會提供一些有關多人遊戲 Manager 正在進行背景。

* [播放與朋友](multiplayer-manager/play-multiplayer-with-friends.md)
* [尋找相符項目](multiplayer-manager/play-multiplayer-with-matchmaking.md)
* [傳送遊戲的邀請](multiplayer-manager/send-game-invites.md)
* [處理通訊協定啟用](multiplayer-manager/handle-protocol-activation.md)

API 的高階概觀見[多人遊戲管理員 API 概觀](multiplayer-manager/multiplayer-manager-api-overview.md)。

## <a name="what-multiplayer-manager-does-not-do"></a>項目多人遊戲管理員不會執行
雖然多人遊戲 Manager 可讓您更容易實作多人遊戲案例，並擷取一些開發人員的資料，有幾件事，它不會處理，或不是最適合。

* 持續性的線上伺服器遊戲，例如 MMOs 或其他需要大型的工作階段 （超過 100 個球員，在工作階段） 的遊戲類型。
* 伺服器工作階段管理的伺服器

>多人遊戲管理員不會繫結至任何特定的網路技術，並應該使用任何網路層。

## <a name="next-steps"></a>後續步驟

請參閱 c + + 或 WinRT*多人遊戲*範例 API 的操作範例。

API 文件位於 Microsoft::Xbox::Services::Multiplayer::Manager 命名空間中的 c + + 或 WinRT 輔助線。  您也可以查看`multiplayer_manager.h`標頭。

如果您有任何疑問、 意見反應，或遇到任何問題，使用多人連線管理員，請連絡您的補西牆，或在論壇上張貼支援執行緒[ https://forums.xboxlive.com ](https://forums.xboxlive.com)。
