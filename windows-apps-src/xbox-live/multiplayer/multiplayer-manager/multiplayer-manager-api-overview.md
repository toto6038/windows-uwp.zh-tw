---
title: 多人遊戲管理員 API 概觀
description: 深入了解 Xbox 即時多人遊戲 Manager API。
ms.assetid: 658babf5-d43e-4f5d-a5c5-18c08fe69a66
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 多人遊戲，多人連線管理員
ms.localizationpriority: medium
ms.openlocfilehash: 7838de6845bc6c49acf649c0e859cd0d7020490f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628023"
---
# <a name="multiplayer-manager-api-overview"></a>多人遊戲管理員 API 概觀

此頁面說明多人遊戲 Manager API，以及它們在遊戲中的使用方式的概觀。 它會呼叫的最重要類別和 API 中的方法。 如需詳細的 API 資訊，請參閱參考文件。 如需如何使用這些 Api 的應用程式中的範例，請參閱多人遊戲範例。

## <a name="namespace"></a>命名空間
多人遊戲 Manager 類別會包含下列命名空間：

| language | 命名空間 |
| --- | --- |
| C++/CX | Microsoft::Xbox::Services::Multiplayer::Manager |
| C++ | xbox::services::multiplayer::manager |

那里下列清單說明您應該了解的主要類別：

* [多人遊戲管理員類別](#multiplayer-manager-class)
* [多人遊戲事件類別](#multiplayer-event-class)
* [多人遊戲成員類別](#multiplayer-member-class)
* [多人遊戲大廳工作階段類別](#multiplayer-lobby-session-class)
* [多人遊戲工作階段類別](#multiplayer-game-session-class)

## <a name="multiplayer-manager-class-a-namemultiplayer-manager-class"></a>多人遊戲管理員類別 <a name="multiplayer-manager-class">

| language | class |
| --- | --- |
| C++/CX | MultiplayerManager |
| C++ | multiplayer_manager |

這個類別是互動與多人遊戲 Manager 的主要類別。 它是單一類別，因此您只需要建立和初始化此類別會一次。
這個類別包含單一大廳工作階段物件和一個單一的遊戲工作階段物件。

至少，您必須呼叫`initialize()`和`do_work()`為了讓多人遊戲管理員函式的這個類別的方法。

下表將說明一些，但不是全部，越常方法與屬性，這個類別所使用。 如需完整描述的清單類別成員，請參閱參考。

| C++/CX | C++ | description |
| --- | --- | --- |
| **方法** | | |
| Initialize （) | initialize （) | 初始化多人連線管理員。 您必須先呼叫這個方法，才能使用多人遊戲管理員。 |
| DoWork() | do_work() | 更新應用程式顯示的工作階段狀態。 您應該呼叫這個方法，至少一次每個畫面，以及您的遊戲應該處理由方法傳回的多人遊戲事件。 |
| JoinLobby() | join_lobby() | 提供用來唯一識別大廳 handleId 透過您想要加入或當使用者接受邀請造成的標題，以取得啟動的通訊協定的朋友的大廳工作階段的聯結。 |
| JoinGameFromLobby() | join_game_from_lobby() | 如果有的話，並有足夠的空間，請加入大廳的遊戲工作階段。 如果工作階段不存在，它會與現有的大廳成員中建立新的遊戲工作階段。 這不會移轉現有大廳工作階段內容移轉至遊戲的工作階段。 加入之後，您可以設定的屬性或透過主機`game_session()::set_synchronized_*`Api。 標題，才能在想要加入此遊戲的工作階段的所有用戶端上呼叫此 API。|
| JoinGame() | join_game() | 加入現有的遊戲工作階段，以指定的全域唯一的工作階段名稱，通常透過第三方配對服務中找到。 您可以傳遞在 xbox 使用者您想要的遊戲的組件的識別碼清單中。|
| FindMatch() | find_match() | 您可以使用 Xbox Live 的配對來尋找及加入遊戲。 |
| LeaveGame() | leave_game() | 離開遊戲，並回到大廳中的成員，所有的本機成員。 |
| **屬性** | | |
| LobbySession | lobby_session() | 物件，表示大廳工作階段控制代碼。 |
| GameSession |  game_session() | 物件，表示在遊戲工作階段控制代碼。 |

## <a name="multiplayer-event-class-a-namemultiplayer-event-class"></a>多人遊戲事件類別 <a name="multiplayer-event-class">

| language | class |
| --- | --- |
| C++/CX | MultiplayerEvent |
| C++ | multiplayer_event |

當您呼叫`do_work()`，多人遊戲管理員會傳回代表工作階段所做的變更，因為您一次呼叫的事件清單`do_work()`。 這些事件包含變更，例如成員已加入工作階段、 成員已離開工作階段、 成員屬性已變更，主應用程式用戶端已變更，等等。

如需所有可能的事件類型的清單，請參閱`multiplayer_event_type`列舉型別。

傳回之每個`multiplayer_event`包含`event_args`，您必須將它轉換成適當 event_args 類別的事件類型。 例如，如果`event_type`是`member_joined`，然後您會將轉換`event_args`參考`member_joined_event_args`類別。

您的遊戲應該處理每個視事件之後呼叫`do_work()`。

## <a name="multiplayer-member-class-a-namemultiplayer-member-class"></a>多人遊戲成員類別 <a name="multiplayer-member-class">

| language | class |
| --- | --- |
| C++/CX | MultiplayerMember |
| C++ | multiplayer_member |

此類別代表大廳或遊戲的工作階段的推手。 此類別包含的成員，包括 XboxUserID 播放程式，播放程式的網路連線位址時，自訂屬性的每個玩家和更多功能的相關屬性。

## <a name="multiplayer-lobby-session-class-a-namemultiplayer-lobby-session-class"></a>多人遊戲大廳工作階段類別 <a name="multiplayer-lobby-session-class">

| language | class |
| --- | --- |
| C++/CX | MultiplayerLobbySession |
| C++ | multiplayer_lobby_session |

持續性的工作階段，用來管理這個裝置，您受邀的朋友想要一起播放的本機使用者。 大廳工作階段必須包含至少一個成員，為了讓多人遊戲管理員採取任何多人遊戲的動作。 您可以呼叫，以一開始建立新的大廳工作階段`add_local_user()`方法。

下表將說明一些，但不是全部，越常方法與屬性，這個類別所使用。 如需完整描述的清單類別成員，請參閱參考。

| C++/CX | C++ | description |
| --- | --- | --- |
| **方法** | | |
| AddLocalUser() | add_local_user() | 將大廳工作階段中的本機使用者 (已登入，在本機裝置的 player)。 如果這是加入大廳工作階段的第一個成員時，它會建立新的大廳工作階段。 |
| RemoveLocalUser() | remove_local_user() | 從大廳和遊戲的工作階段中移除指定的成員。 |
| InviteFriends() | invite_friends() | 開啟標準 Xbox Live UI 可讓使用者從清單中選取其朋友，播放程式，然後邀請至遊戲的玩家。 |
| InviteUsers() | invite_users() | 邀請遊戲 sepcified Xbox Live 使用者。 |
| SetLocalMemberConnectionAddress() | set_local_member_connection_address() | 設定本機成員的網路位址。 遊戲可以使用此網路位址，來建立成員之間的網路通訊。 |
| SetLocalMemberProperties() | set_local_member_properties() | 設定本機成員的自訂屬性。 屬性會儲存在 JSON 字串。 |
| DeleteLocalMemberProperties() | delete_local_member_properties() | 本機成員中移除自訂屬性。 |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | 大廳工作階段設定的自訂屬性。 屬性會儲存在 JSON 字串。 如果屬性在裝置之間共用，且在同一時間可能會有數個裝置更新，請使用方法的同步處理的版本。 |
| IsHost() | is_host() | 表示是否目前的裝置做為大廳主應用程式。 |
| SetSynchronizedHost() | set_synchronized_host() | 設定主機的入口。 |
| **屬性** | | |
| LocalMembers | local_members() | 在本機裝置登入的成員的集合。 |
| 成員 | members() | 大廳工作階段中的成員的集合。 |
| 屬性 | properties （) | 表示大廳工作階段的屬性集合的 JSON 物件。 |
| 主機 | host() | 大廳主機成員。 |


## <a name="multiplayer-game-session-class-a-namemultiplayer-game-session-class"></a>多人遊戲工作階段類別 <a name="multiplayer-game-session-class">

| language | class |
| --- | --- |
| C++/CX | MultiplayerGameSession |
| C++ | multiplayer_game_session |

遊戲的工作階段代表實際的遊戲的執行個體中的 Xbox Live 成員的群組。 這可能包括透過配對服務相符的播放程式。

若要啟動新的遊戲工作階段，其中包含從成員`lobby_session`，您可以呼叫`multiplayer_manager::join_game_from_lobby()`。 如果您想要使用 Xbox Live 的配對，您可以呼叫`multiplayer_manager::find_match()`。 如果您使用第三方配對服務，您可以呼叫`multiplayer_manager::join_game()`。

下表將說明一些，但不是全部，越常方法與屬性，這個類別所使用。 如需完整描述的清單類別成員，請參閱參考。

| C++/CX | C++ | description |
| --- | --- | --- |
| **方法** | | |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | 設定遊戲工作階段的自訂屬性。 屬性會儲存在 JSON 字串。 如果屬性在裝置之間共用，且在同一時間可能會有數個裝置更新，請使用方法的同步處理的版本。 |
| IsHost() | is_host() | 表示是否目前的裝置做為遊戲的主機。 |
| SetSynchronizedHost() | set_synchronized_host() | 設定主機的遊戲。 |
| **屬性** | | |
| 成員 | members() | 在遊戲工作階段中的成員的集合。 |
| 屬性 | properties （) | 代表遊戲的工作階段的屬性集合的 JSON 物件。 |
| 主機 | host() | 遊戲主機成員。 |
