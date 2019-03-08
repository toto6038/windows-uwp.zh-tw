---
title: 使用 XIM （c + +）
description: 了解如何使用 c + + 中的 Xbox 整合式多人遊戲 (XIM)。
ms.date: 04/24/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 一個 xbox、 xbox 整合式多人遊戲
ms.localizationpriority: medium
ms.openlocfilehash: 798d1a1bc738cbdc7bb2b3fb34076f0897fc76ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644053"
---
# <a name="using-xim-c"></a>使用 XIM （c + +）

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

這是使用 XIM 的 c + + API 的簡短逐步解說。 遊戲開發人員想要存取 XIM 透過C#應該會看到[使用 XIM (C#)](using-xim-cs.md)。

- [使用 XIM （c + +）](#using-xim-c)
    - [必要條件](#prerequisites)
    - [初始化和啟動](#initialization-and-startup)
    - [非同步作業和處理狀態變更](#asynchronous-operations-and-processing-state-changes)
    - [處理 XIM 播放程式物件](#handling-xim-player-objects)
    - [啟用參加的朋友，並邀請他們](#enabling-friends-to-join-and-inviting-them)
    - [基本的配對，並移至另一個 XIM 網路與其他人](#basic-matchmaking-and-moving-to-another-xim-network-with-others)
    - [停用配對 NAT 規則，以進行偵錯](#disabling-matchmaking-nat-rule-for-debugging-purposes)
    - [離開 XIM 網路，以及清除](#leaving-a-xim-network-and-cleaning-up)
    - [傳送和接收訊息](#sending-and-receiving-messages)
    - [評估資料路徑品質](#assessing-data-pathway-quality)
    - [使用播放程式的自訂屬性的資料共用](#sharing-data-using-player-custom-properties)
    - [使用網路的自訂屬性的資料共用](#sharing-data-using-network-custom-properties)
    - [使用每個播放程式技能的配對](#matchmaking-using-per-player-skill)
    - [使用每個播放程式角色的配對](#matchmaking-using-per-player-role)
    - [XIM 與播放程式團隊的運作方式](#how-xim-works-with-player-teams)
    - [使用對談](#working-with-chat)
    - [靜音播放程式](#muting-players)
    - [設定使用播放程式團隊的交談目標](#configuring-chat-targets-using-player-teams)
    - [自動背景填滿的 player 位置 （「 回填 」 配對）](#automatic-background-filling-of-player-slots-backfill-matchmaking)
    - [查詢可聯結的網路](#querying-joinable-networks)

## <a name="prerequisites"></a>必要條件

開始使用 XIM 撰寫程式碼之前，有兩個必要條件。

1. 您必須已設定您的應用程式 AppXManifest 的標準多人連線的網路功能，您必須已設定的網路資訊清單部分宣告所需的流量模式 XIM 所使用的範本。

    AppXManifest 功能和網路資訊清單所述的平台的文件; 其個別區段中的更多詳細資料典型的 XIM 特定 XML，若要貼上不須[XIM 專案組態](xim-manifest.md)。

1. 您必須有兩個可用的應用程式身分識別資訊：

    * 指派的 Xbox Live 標題的識別碼。
    * 提供佈建您的應用程式存取 Xbox Live 服務的服務組態識別碼。

    取得這些，請參閱您的 Microsoft 代表，如需詳細資訊。 在初始化期間，將會使用這項資訊。

編譯 XIM 需要包括主要`XboxIntegratedMultiplayer.h`標頭。 若要正確連結，您的專案也必須包含`XboxIntegratedMultiplayerImpl.h`中至少一個的編譯單位 （因為這些虛設常式的函式實作都是小型且容易編譯器產生為 「 內嵌 」，常見的先行編譯標頭建議選項）。

中所述[XIM 概觀](../xbox-integrated-multiplayer.md)，XIM 介面不需要的專案，選擇 編譯以 C + + /CX 與傳統的 c + +，它可與其中一個。 實作也不會擲回例外狀況做為報告，所以您可以使用它輕鬆地從無例外狀況的專案中，如果慣用的非嚴重錯誤。

## <a name="initialization-and-startup"></a>初始化和啟動

您開始進行互動的程式庫初始化 XIM 物件單一物件，與您的 Xbox Live 服務組態識別碼字串和標題的識別碼。 如果您同時使用 Xbox 服務 API，您可能會發現它方便擷取從`xbox::services::xbox_live_app_config`。 下列範例假設這些值已經位於 'myServiceConfigurationId' 和 'myTitleId' 變數分別：

```cpp
xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
```

初始化之後，應用程式應該擷取所有目前在本機裝置上的使用者參與，並將其傳遞到 Xbox 使用者識別碼字串`xim::set_intended_local_xbox_user_ids()`呼叫。 下列範例程式碼會假設單一使用者已按下控制器按鈕，表示要播放的意圖，而且與使用者相關聯的 Xbox 使用者識別碼字串已擷取到 'myXuid' 變數：

```cpp
xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
```

呼叫`xim::set_intended_local_xbox_user_ids()`立即設定本機使用者，應該將 XIM 網路相關聯的 Xbox 使用者識別碼。 這份 Xbox 使用者識別碼將用於所有未來的網路作業清單變更到另一個呼叫為止`xim::set_intended_local_xbox_user_ids()`。

在沒有 XIM 網路在完全尚未存在的情況下，第一個步驟是移至 XIM 網路，以取得啟動該處理程序。 如果使用者還沒有特定的 XIM 網路記住，最佳做法是只將移動到新的、 空的網路。 這可讓使用者的朋友，要加入做為一種 「 大廳"從中它們可以一起共同作業來選取他們下一步 的多人遊戲活動 （例如，輸入配對） 的網路。

以下是如何移動只是本機使用者與先前新增的範例`xim::set_intended_local_xbox_user_ids()`來容納最多 8 總共的播放程式的空白 XIM 網路：

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_only_local_players);
```

若要呼叫`xim::move_to_new_network()`起始非同步移動於結尾提供開發人員應該定期程序的狀態變更事件的作業。

## <a name="asynchronous-operations-and-processing-state-changes"></a>非同步作業和處理狀態變更

XIM 的核心是應用程式的規則，經常呼叫`xim::start_processing_state_changes()`和`xim::finish_processing_state_changes()`一組方法。 這些方法是 XIM 方式通知應用程式已準備好處理多人遊戲狀態、 更新和 XIM 如何提供這些更新。 它們可以快速地操作，使它們可以在您的 UI 轉譯迴圈中呼叫每個圖形畫面格。 這提供方便的地方，而不需擔心網路時間或複雜的多執行緒的回呼的不可預測性擷取所有佇列中的變更。 XIM API 實際上最適合此單一執行緒模式。 它可以保證其狀態會保持不變，這兩個函數，外部，因此您可以直接且有效率地使用它。

基於相同理由，應該 XIM API 所傳回的所有物件*不*視為安全執行緒。 媒體櫃有內部的多執行緒保護，但您仍必須實作您自己鎖定，如果您需要任意的執行緒存取任何 XIM 的值。 比方說，如果有一個執行緒逐步`xim::players()`清單，而另一個執行緒可能會叫用其中一個`xim::start_processing_state_changes()`或`xim::finish_processing_state_changes()`和改變該播放清單相關聯的記憶體。

當`xim::start_processing_state_changes()`是呼叫，所有已排入佇列的更新中所報告的陣列`xim_state_change`結構的指標。

應用程式應該：

1. 逐一查看陣列。
1. 檢查其更特定類型的基底結構。
1. 基底結構類型轉換至對應的詳細型別。
1. 處理視需要更新。

一旦完成所有`xim_state_change`目前可用的結構，該陣列應該傳遞回來釋放資源，藉由呼叫 XIM `xim::finish_processing_state_changes()`。 例如：

```cpp
uint32_t stateChangeCount;
xim_state_change_array stateChanges;
xim::singleton_instance().start_processing_state_changes(&stateChangeCount, &stateChanges);
for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; stateChangeIndex++)
{
   const xim_state_change * stateChange = stateChanges[stateChangeIndex];
   switch (stateChange->state_change_type)
   {
       case xim_state_change_type::player_joined:
       {
           MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change*>(stateChange));
           break;
        }

       case xim_state_change_type::player_left:
       {
           MyHandlePlayerLeft(static_cast<const xim_player_left_state_change*>(stateChange));
           break;
       }

       ...
    }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

既然您已基本處理迴圈時，您可以處理初始與相關聯的狀態變更`xim::move_to_new_network()`作業。 每個 XIM 網路移動作業的開頭`xim_move_to_network_starting_state_change`。 如果因為任何原因而失敗的移動，則您的應用程式將會提供`xim_network_exited_state_change`，這是常見的失敗處理機制，防止您移動至 XIM 網路，或您中斷目前 XIM 網路任何非同步錯誤。 否則，請移動會完成，但`xim_move_to_network_succeeded_state_change`已完成所有的狀態，且所有播放程式已成功新增至 XIM 網路之後。

當使用者未具備玩多人連線的工作階段中的其他人的平台特殊權限時，XIM 會傳送給嘗試建立或加入 XIM 網路裝置`xim_network_exited_state_change`原因`xim_network_exit_reason::user_multiplayer_restricted`。 使用本機使用者的任何設定時，發生這種的情況`xim::set_intended_local_xbox_user_ids()`有 Xbox Live 的多人連線限制。 使用 XIM 的時代的 Xbox 應用程式應該呼叫`Store::Product::CheckPrivilegeAsync`在本機的播放程式再將它們移到 attemptResolution 組 XIM 網路中，為 true，或在接收到回應中這麼做`xim_network_exit_reason::user_multiplayer_restricted`傳回的原因為`xim_network_exited_state_change`。

## <a name="handling-xim-player-objects"></a>處理 XIM 播放程式物件

假設範例中，移到新的 XIM 網路的單一本機使用者的成功，您的應用程式將會提供`xim_player_joined_state_change`本機`xim_player`物件。 只要該播放程式執行個體本身是有效，此物件的指標會保持有效。 播放程式執行個體會變成無效時對應`xim_player_left_state_change`該播放程式執行個體已透過呼叫傳回`xim::finish_processing_state_changes()`。 您的應用程式一定會提供`xim_player_left_state_change`針對每個`xim_player_joined_state_change`。

您也可以擷取所有的陣列`xim_player`XIM 中的物件使用網路隨時`xim::get_players()`。

`xim_player`物件有許多實用的方法，例如`xim_player::gamertag()`擷取目前的 Xbox Live 的玩家代號字串與用於顯示播放器相關聯。 如果`xim_player`是本機裝置，則它也會報告非 null`xim_player::xim_local`物件指標，從`xim_player::local()`，其中包含本機的播放程式僅可供使用的其他方法。

當然，播放程式的最重要狀態不知道 XIM 的一般資訊，但您特定的應用程式想要追蹤。因為您可能會有自己建構，該追蹤資訊，您會想要連結`xim_player`物件複製到您自己的播放程式建構物件，以便隨時 XIM 報表`xim_player`，您可以快速擷取而不需要填入您的狀態和查閱自訂播放程式的內容指標。 下列範例假設您的私用狀態的指標是在變數 'myPlayerStateObject' 和新加入`xim_player`物件處於 'newXimPlayer' 的變數：

```cpp
newXimPlayer->set_custom_player_context(myPlayerStateObject);
```

這會將指定的指標值儲存在本機的播放程式物件 （它永遠不會透過網路傳送到遠端裝置，記憶體不會有效）。 然後您將可以一直回到您的物件擷取自訂的內容，並將它轉換您的物件，如下列範例所示：

```cpp
myPlayerStateObject = reinterpret_cast<MyPlayerState *>(newXimPlayer->custom_player_context());
```

您可以變更指派給自訂播放程式內容指標`xim_player`在任何時間。

## <a name="enabling-friends-to-join-and-inviting-them"></a>啟用參加的朋友，並邀請他們

隱私權和安全性，所有新的 XIM 網路會自動依預設會設定為只可加入本機的播放器，而且完全由應用程式，以明確允許其他人，當其就緒。 下列範例示範如何使用`xim::network_configuration()`擷取目前的網路設定，並更新 joinability 使用`xim::set_network_configuration()`開始讓播放程式，以加入新本機使用者，以及其他使用者已受邀，或被 「後面接著"（Xbox Live 社交關係） 已經 XIM 網路中的播放程式：

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance.network_configuration();
networkConfiguration.allowed_player_joins = xim_allowed_player_joins::local | xim_allowed_player_joins::invited | xim_allowed_player_joins::followed;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

`xim::set_network_configuration()` 執行 asynchronoulsy。 當先前的程式碼範例呼叫完成時，`xim_network_configuration_changed_state_change`可用來通知應用程式，joinability 值已變更其預設值是從`xim_allowed_player_joins::none`。 您可以接著查詢新值，藉由檢查`allowed_player_joins`的屬性`xim_network_configuration`所傳回`xim::network_configuration()`。 

`allowed_player_joins` XIM 網路，以判斷網路的 joinability 裝置時可檢查。

其中一個本機的播放程式應該要送出給遠端使用者的邀請加入此 XIM 網路，應用程式可以呼叫`xim_player::xim_local::show_invite_ui()`啟動系統邀請 UI。 在這裡，本機使用者可以選取他們想要邀請，並送出邀請的人員。 下列範例會示範其運作方式，並假設變數 'ximPlayer' 是否指向有效的本機`xim_player`:

```cpp
ximPlayer->local()->show_invite_ui();
```

在上面的陳述式執行之後，系統邀請 UI 將會顯示。 使用者已傳送邀請 （或否則關閉 UI），一旦`xim_show_invite_ui_completed_state_change`下次呼叫將會收到`xim::start_processing_state_changes()`。 或者，您的應用程式可以傳送的邀請，直接使用`xim_player::xim_local::invite_users()`不會顯示系統邀請 UI。

無論如何，遠端使用者會收到 Xbox Live 的邀請郵件每當使用者登入，他們可以選擇接受或拒絕它。 如果他們接受，通訊協定啟動會起始您的應用程式，這些裝置上，如果它沒有執行，且通訊協定啟動會傳遞至您的應用程式，可用來將移至對應的 XIM 網路及其事件引數。

在 通訊協定啟動自己，請參閱平台文件，如需詳細資訊。

下列範例會示範如何呼叫`xim::extract_protocol_activation_information()`判斷是否適用於 XIM 事件引數。 這是假設您所擷取的原始 URI 字串`Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs`'uriString' 的變數：

```cpp
xim_protocol_activation_information activationInfo;
bool isXimActivation;
isXimActivation = xim::singleton_instance().extract_protocol_activation_information(uriString, &activationInfo);
```

如果是 XIM 啟動過程中，則您會想要確定已填入的 'local_xbox_user_id' 欄位所識別的本機使用者`xim_protocol_activation_information`結構登入，且是在指定的使用者之中`xim::set_intended_local_xbox_user_ids()`。 您可以使用的呼叫將移至指定的 XIM 網路起始`xim::move_to_network_using_protocol_activated_event_args()`使用相同的 URI 字串。 例如：

```cpp
xim::singleton_instance().move_to_network_using_protocol_activated_event_args(uriString);
```

也請注意，「 遵循 」 的遠端使用者可以瀏覽至 本機使用者的播放程式卡中的系統 UI，並自行起始聯結嘗試，而不需要邀請 （假設您已允許這類 player 聯結，如上所示）。 此動作會也通訊協定啟動您的應用程式，就像邀請作業，並會以相同方式進行處理。

移至使用通訊協定啟用的 XIM 網路等同於移動到新的 XIM 網路中所示[初始化和啟動](#initialization-and-startup)。 唯一的差別在於，當成功移動，移動的裝置會提供本機和遠端的播放程式`xim_player_joined_state_change`結構表示所有適用的播放程式。 當然，原始裝置已存在於 XIM 網路不會移動，但會看到新裝置的使用者新增為透過其他玩家`xim_player_joined_state_change`結構。

一旦 XIM 網路中，跨不同裝置的播放程式會結合在一起，它們可以起始多人遊戲情節、 透過語音和文字自動進行通訊，並將傳送應用程式特定的訊息。

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>基本的配對，並移至另一個 XIM 網路與其他人

將播放程式移至也有陌生人 XIM 網路中，您可以進一步展開朋友們會互相的網路體驗。 陌生人是玩家從世界各地會一起使用 Xbox Live 的配對服務，而此一根據類似的興趣。

起始與 XIM 基本配對的簡單方式是藉由呼叫`xim::move_to_network_using_matchmaking()`其中一個填入的裝置上`xim_matchmaking_configuration`結構中，取得從目前的 XIM 網路，以及它的播放程式。

下列範例會起始移動，使用配對組態設定來尋找總共 8 播放器無小組戰鬥相符項目。 如果找不到 8 總共的播放程式，系統可能仍需符合 2-7 玩家在一起。 此範例會使用應用程式定義常數，型別、 vector<pair<uint64_t、 名為 MYGAMEMODE_DEATHMATCH，表示要關閉的篩選的遊戲模式。 XIM 的配對只會符合此網路與其他玩家指定相同值，並提供第二個參數時，會沿著從目前的 XIM 網路的所有社交加入玩家`xim_players_to_move::bring_existing_social_players`至`xim::move_to_network_using_matchmaking()`:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

像先前的移動，此配對程序會提供初始`xim_move_to_network_starting_state_change`所有的裝置上和`xim_move_to_network_succeeded_state_change`之後移動作業順利完成。 由於這是從一個 XIM 網路移至另一個，一項差異是，已已經有`xim_player`物件新增為本機和遠端使用者，和這些將繼續一起移至新的 XIM 網路的所有播放程式。 在播放程式之間的對談和資料通訊會繼續進行配對時運作不中斷。

配對可以是耗時的程序，根據潛在配對集區中的播放程式也具有稱為數目`xim::move_to_network_using_matchmaking()`。

A`xim_matchmaking_progress_updated_state_change`會定期提供整個作業，保持您和使用者目前的狀態。 其他播放程式時已找到相符項目，會加入至一般 XIM 網路`xim_player_joined_state_change`和移動完成。

一旦您完成 「 matchmade"玩家這組的多人遊戲體驗，您可以重複移轉至不同的 XIM 網路與另一回合的配對的程序。 您會看到先前透過已加入每個玩家`xim::move_to_network_using_matchmaking()`作業提供`xim_player_left_state_change`表示其`xim_player`物件已不在相同的 XIM 網路中。

如果當您選擇以起始一次使用配對`xim::move_to_network_using_matchmaking()`，您指定`xim_players_to_move::bring_existing_social_players`，透過社交的進入點已加入的播放程式 (`xim::move_to_network_using_protocol_activated_event_args()`或`xim::move_to_network_using_joinable_xbox_user_id()`) 進行新的配對時，會保留。 或者，指定`xim_players_to_move::bring_only_local_players`會中斷連線的社交連線遠端的玩家，離開只保留本機參與者。 在這兩種情況下，第二個移動作業完成時將會加入一組不同的陌生人。

您也可以選擇移動非 matchmade 播放程式 （或只是本機的播放器） 以全新的 XIM 網路決定的下一個配對組態/多人遊戲活動。 下列範例示範如何呼叫裝置一樣`xim::move_to_new_network()`的上限為 8 的播放程式，但這次將採取的現有社交加入播放程式也 XIM 網路：

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_existing_social_players);
```

A`xim_move_to_network_starting_state_change`並`xim_move_to_network_succeeded_state_change`會提供給所有參與的裝置，連同`xim_player_left_state_change`留下 matchmade 玩家。 在這些裝置，它們會同樣地看到`xim_player_left_state_change`移出網路的每個播放程式。

您可以繼續從 XIM 網路移至 XIM 網路許多次，視上面所述的方式。

針對效能、 Xbox Live 服務不會嘗試比對的播放程式不太可能能夠建立任何直接的對等連線的裝置上的群組。 移至新的網路使用配對作業如果您正在開發中未正確設定為支援標準 Xbox Live 多人連線的網路環境，可能會無限期繼續沒有成功相符，即使在您確定您有足夠的玩家符合配對準則所有移動，全部都使用相同的本機環境中的裝置。 請務必執行中的網路設定區域/Xbox 應用程式的多人連線測試，並遵循其建議，如果它會報告問題，特別是關於"Strict NAT"。

## <a name="disabling-matchmaking-nat-rule-for-debugging-purposes"></a>停用配對 NAT 規則，以進行偵錯

如果您的網路系統管理員而無法進行必要的環境變更，以支援 XIM 的 NAT 規則，您可以解除封鎖您測試設定以便比對"Strict NAT"裝置，而不需要至少一個 開啟 XIM Xbox One 的開發套件的配對NAT"裝置。 這是檔案，稱為 「 xim_disable_matchmaking_nat_rule"（內容不重要），加上所有的 Xbox One 主控台的 「 標題從頭開始"磁碟機的根目錄。 若要這麼做的範例方法之一是 XDK 的命令提示字元執行下列命令再啟動您的應用程式，每個主控台適當地取代"{console_name_or_ip_address}"預留位置：

```bat
echo.>%TEMP%\emptyfile.txt
copy %TEMP%\emptyfile.txt \\{console_name_or_ip_address}\TitleScratch\xim_disable_matchmaking_nat_rule
del %TEMP%\emptyfile.txt
```

> [!NOTE]
> 此開發因應措施是目前僅適用於 Xbox One 獨佔資源應用程式，並不支援通用 Windows 應用程式。 也請注意，主控台會使用這項設定，將一律無法符合與裝置，也不具備此檔案存在，不論網路環境中，因此請務必新增和移除所有位置的檔案。

## <a name="leaving-a-xim-network-and-cleaning-up"></a>離開 XIM 網路，以及清除

完成本機使用者參與 XIM 網路，通常只會移到新的 XIM 網路，以允許本機使用者，受邀使用者，並 「 遵循 」 使用者加入讓他們可以繼續協調與他的朋友，尋找下一個活動。 但如果使用者完全利用所有的多人遊戲體驗，則您的應用程式可能會想要離開 XIM 完全網路，並如同只有傳回的狀態`xim::initialize()`和`xim::set_intended_local_xbox_user_ids()`呼叫。 這是使用`xim::leave_network()`方法：

```cpp
xim::singleton_instance().leave_network();
```

這個方法會開始以非同步方式正常地從其他參與者中斷連線處理程的序。 這會導致要提供的遠端裝置`xim_player_left_state_change`每個本機玩家中斷連線。 在本機裝置會也會提供`xim_player_left_state_change`的每個本機和遠端的播放程式，已中斷連線。 當所有中斷連線作業完成時，最終`xim_network_exited_state_change`會提供。 應用程式接著可以呼叫 'xim::cleanup()' 來釋放所有資源，並返回 未初始化的狀態：

```cpp
xim::singleton_instance().cleanup();
```

叫用`xim::leave_network()`並等候`xim_network_exited_state_change`以結束 XIM 網路依正常程序一律強烈建議您當`xim_network_exited_state_change`已經尚未提供。

如果`xim::cleanup()`而不直接呼叫`xim::leave_network()`呼叫，則剩餘的參與者的通訊效能問題將會發生，因為它們將會強制將無法傳遞的訊息傳送至只是 「 消失 」 的裝置呼叫`xim::cleanup()`。

## <a name="sending-and-receiving-messages"></a>傳送和接收訊息

XIM 和其基礎的元件會執行所有單調乏味的工作，透過網際網路建立安全的通訊通道，因此您不必擔心連線問題或無法連線到部分而非全部的播放程式。 如果有任何基本的端對端連線能力問題，請移至 XIM 網路將不會成功。

格式正確且確認與 XIM 網路時`xim_move_to_network_succeeded_state_change`，以確實掌握的保證您的應用程式，所有已加入的裝置上的所有執行個體每`xim_player`連線到 XIM 網路，且可以將訊息傳送至其中任何。

讓我們示範如何透過 XIM 網路傳送訊息。 下列範例會假設 'sendingPlayer' 變數是有效的本機 player 物件的指標。 'sendingPlayer' 會叫用`local()`並呼叫`send_data_to_other_players()`到所有播放程式 （本機和遠端） 保證的循序傳遞傳送的訊息結構 'msgData'。 請注意它移到所有播放程式，因為特定的播放程式的清單未傳遞至方法：

```cpp
sendingPlayer->local()->send_data_to_other_players(sizeof(msgData), &msgData, 0, nullptr, xim_send_type::guaranteed_and_sequential);
```

將訊息傳遞到所有播放程式使用`send_data_to_other_players()`不特定的玩家的陣列傳遞至方法。

提供訊息的所有收件者`xim_player_to_player_data_received_state_change`，以及指向對應 xim_player 物件傳送它，並在本機收到它包含一份資料的指標。

當然，保證循序傳遞很方便，但它也可以是效率不佳的傳送型別，因為 XIM 需要重新傳輸或延遲的訊息，如果卸除或得紊亂不堪以網際網路封包。 請務必考慮使用其他的傳送類型為您的應用程式可以容忍遺失或需要的訊息不按順序到達。 由於訊息資料來自遠端電腦時，最佳做法是清楚定義的資料格式，例如封裝以特定的位元組順序 （「 字節順序） 的多位元組值。 應用程式也應該驗證資料，然後才在其上運作。

XIM 提供網路層級安全性，因此不需要額外的加密或簽章配置。 不過，我們建議一律採用 「-深度防禦 」，以防止意外的應用程式錯誤，並能處理您的應用程式依正常程序 （在開發、 內容更新等） 的通訊協定共存的不同版本。 使用者的網際網路連線可以是有限且不斷變化的資源。 我們強烈建議使用有效率的訊息資料格式和避免傳送每個 UI 畫面格的設計。

## <a name="assessing-data-pathway-quality"></a>評估資料路徑品質

若要深入了解目前的資料路徑，讓兩位玩家的品質，呼叫`xim_player::network_path_information()`方法。 下列範例會擷取指標`xim_network_path_information`結構`xim_player`'remotePlayer' 變數中所包含的指標：

```cpp
 const xim_network_path_information * networkPathInfo = remotePlayer->network_path_information();
```

傳回的結構包含預估的來回行程延遲和多少訊息仍在佇列本機傳輸案例中的連接不支援該時間點的傳輸更多資料。

如果您看到佇列要備份特定 'xim_player'，您應該減少在您要傳送資料的速率。

`xim_network_path_information::round_trip_latency_in_milliseconds`欄位代表的基礎網路延遲和 XIM 的估計的延遲而不需要佇列。 有效的延遲會隨著`xim_network_path_information::send_queue_size_in_messages`成長和 XIM 適用於透過佇列。

選擇要開始節流呼叫的合理點`send_data_to_other_players`根據該遊戲的使用狀況和需求。 傳送佇列中的每個訊息代表有效的網路延遲增加。

接近 XIM 的最大限制 （目前為 3500 訊息） 值太高，適用於大多數的遊戲，並可能表示等待根據呼叫的速率傳送資料的幾秒鐘`send_data_to_other_players`和每個資料承載大小。 相反地，選擇 數字，會考量該遊戲的延遲需求，以及如何抖動遊戲`send_data_to_other_players`呼叫的模式。

## <a name="sharing-data-using-player-custom-properties"></a>使用播放程式的自訂屬性的資料共用

大部分的應用程式資料交換進行`xim_player::xim_local::send_data_to_other_players()`方法，因為它允許的最大控制誰會收到資料，當他們應該會收到該資料，以及系統如何處理封包遺失。 不過有的時間時，這就非常有用的播放器本身的相關基本，很少變更的狀態與其他人分享最少的複雜。 比方說，每個播放程式可能有固定的字串，表示字元已選取該模型後，再進入多人遊戲，所有播放程式用來呈現其遊戲中表示法。

不會經常變更播放程式的相關的資料，XIM 會提供播放程式的自訂屬性。 這些屬性是由應用程式定義名稱和值，也就是，可以套用至本機的播放程式，並可自動傳播到所有裝置變更時，null 結尾字串組所組成。

自訂 player 屬性有多個執行內部同步處理額外負荷比`xim_player::xim_local::send_data_to_other_players()`方法，以便針對快速變更的資料 （也就是播放程式的位置），您仍然應該將直接傳送。

播放程式的自訂屬性索引鍵 / 值組具有其目前的值，這些裝置加入 XIM 網路，並查看新增的播放程式時，會自動提供給新參與的裝置。 可以設定值，藉由呼叫`xim_player::xim_local::set_player_custom_property()`使用名稱和值的字串。 在下列範例中，我們設定一個名為 「 模型 」 有 「 暴力密碼破解 」 的值，在本機屬性`xim_player`指向的變數 'localPlayer' 物件：

```cpp
localPlayer->local()->set_player_custom_property(L"model", L"brute");
```

播放程式內容的變更會使`xim_player_custom_properties_changed_state_change`提供給所有的裝置，警示他們的已變更的屬性名稱。 可以使用任何的播放程式、 本機或遠端，擷取指定之名稱的值`xim_player::get_player_custom_property()`。 下列範例會擷取名為 「 模型 」 屬性的值從`xim_player`指向 'ximPlayer' 的變數：

```cpp
PCWSTR modelName = ximPlayer->get_player_custom_property(L"model");
```

設定指定的屬性名稱的新值將會取代任何現有的值。 將屬性名稱設定為 null 值的字串指標的值都會被視為與空值字串相同，也就是不具有已指定尚未屬性相同。 名稱和值不由解譯 XIM，因此會維持應用程式，可依需要驗證的字串內容。

從一個 XIM 網路移至另一個時，一律會重設自訂播放程式內容。 不過，新的播放程式加入 XIM 網路會收到所有的玩家都有一些組目前的自訂播放程式內容。

## <a name="sharing-data-using-network-custom-properties"></a>使用網路的自訂屬性的資料共用

網路的自訂屬性是為了方便起見特有 XIM 網路也不會經常變更的資料同步處理。 相同網路的自訂屬性的工作[播放程式的自訂屬性](#sharing-data-using-player-custom-properties)，但它們 XIM 單一物件上設定`xim::set_network_custom_property()`。 下列範例會設定具有值"要塞 」 的 「 對應 」 屬性：

```cpp
xim::singleton_instance().set_network_custom_property(L"map", L"stronghold");
```

變更網路屬性會使`xim_network_custom_properties_changed_state_change`提供給所有的裝置，警示他們的已變更的屬性名稱。 指定名稱的值可以使用擷取`xim::get_network_custom_property()`，例如在下列範例會擷取屬性具名 「 對應 」 的值：

```cpp
PCWSTR mapName = xim::singleton_instance().get_network_custom_property(L"map");
```

就如同[播放程式的自訂內容](#sharing-data-using-player-custom-properties)，為指定的屬性名稱將會取代任何現有的值設定新值。 將屬性名稱設定為 null 值的字串指標的值都會被視為與空值字串相同，也就是不具有已指定尚未屬性相同。 名稱和值不由解譯 XIM，因此會維持應用程式，可依需要驗證的字串內容。

新建立的 XIM 網路一律啟動且不需要網路的自訂屬性設定。 不過，新的播放程式加入現有的 XIM 網路將會收到該 XIM 網路設定的網路自訂屬性的目前值。

## <a name="matchmaking-using-per-player-skill"></a>使用每個播放程式技能的配對

由共同興趣的特定應用程式指定遊戲模式比對播放程式是個不錯的基底策略。 可用的播放程式的集區增加時，您應該考慮也比對播放程式，讓經驗豐富的播放程式可以享有與其他的老手，狀況良好的競賽的挑戰，而較新的播放程式可以增加，根據使用者的個人技能或與您的遊戲體驗在其他競爭，類似的功能。

若要這樣做，一開始先在他們的每個播放程式的角色和技能的呼叫中指定的組態結構的所有本機玩家提供的技術等級`xim_player::xim_local::set_roles_and_skill_configuration()`之前開始移往 XIM 網路使用配對。 技能層級是應用程式特有的概念和數字無法加以解譯 XIM，不同之處在於會先嘗試尋找具有相同的技能值，播放程式配對，並將其定期擴大 + /-10，以嘗試尋找宣告技能其他玩家的遞增量為其搜尋該技能周圍範圍內的值。 下列範例假設本機`xim_player`物件，其指標為 'localPlayer'，具有相關聯的應用程式專屬 uint32_t 技能值從本機或 Xbox Live 的儲存體擷取到一個名為 'playerSkillValue' 變數：

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.skill = playerSkillValue;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

完成此動作，將會提供所有參與者都`xim_player_roles_and_skill_configuration_changed_state_change`指出這`xim_player`它的每個播放程式的角色和技能組態已變更。 新的值可以藉由呼叫擷取`xim_player::roles_and_skill_configuration()`。 當所有播放程式會有非 null 的角色和技能套用的設定時，您可以將它移到 XIM 網路使用的值為 true 的配對`require_player_roles_and_skill_configuration`欄位`xim_matchmaking_configuration`若要指定結構`xim::move_to_network_using_matchmaking()`。 下列範例會填入無小組戰鬥，使用與其他玩家值只會比對的 MYGAMEMODE_DEATHMATCH 所定義的常數、 vector<pair<uint64_t 應用程式特定的遊戲模式中找到 2-8 播放程式總數的配對設定指定相同值，而且需要每位玩家角色及技術組態：

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

此結構會提供給`xim::move_to_network_using_matchmaking()`，移動作業會開始通常只要呼叫移動玩家`xim_player::xim_local::set_roles_and_skill_configuration()`為非 null`xim_player_roles_and_skill_configuration`指標。 如果未指定任何播放程式，則配對程序將會暫停，而且會提供所有參與者都`xim_matchmaking_progress_updated_state_change`與`xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration`值。 這包括播放程式後續加入 XIM 網路透過先前傳送的邀請，或透過其他方式 (例如，若要呼叫`xim::move_to_network_using_joinable_xbox_user_id()`) 配對完成之前。 一旦所有播放程式提供其`xim_player_roles_and_skill_configuration`結構配對將會繼續。

使用每個播放程式技能的配對也與配對使用者每個播放程式的角色，結合下, 一節中所述。 如果只有一個想要您可以指定其他的值為 0。 這是因為他們擁有的所有宣告的播放程式`xim_player_roles_and_skill_configuration`技能值為 0 會一律彼此相符。

一次`xim::move_to_network_using_matchmaking()`或其他任何 XIM 網路移動作業完成後，所有的玩家`xim_player_roles_and_skill_configuration`結構將會自動清除為 null 指標 (及隨附的`xim_player_roles_and_skill_configuration_changed_state_change`通知)。 如果您打算將移至另一個使用需要每個播放器組態的配對的 XIM 網路，您必須呼叫`xim_player::xim_local::set_roles_and_skill_configuration()`再次使用新的結構指標，包含最新的資訊。

## <a name="matchmaking-using-per-player-role"></a>使用每個播放程式角色的配對

使用每位玩家角色和技能組態來改善使用者的配對經驗的另一個方法是透過必要的播放程式的角色使用。 這是最適合遊戲，並提供可選取的字元類型，以不同的合作式 play 樣式;也就是類型，不只是改變遊戲中的圖形表示法，但控制互補，開發具有影響力的屬性，例如防禦"治療 」 與近"近戰 」 犯罪與較遠的 「 範圍 」 攻擊的支援。 使用者的人物代表它們可能會想要播放特定的特製化。 但如果您的遊戲的設計可讓不在功能上可能沒有完成每個角色至少一個人的情況下完成目標，有時最好是以符合這類比要比對任何玩家一起然後放在一起的第一個要求來交涉的播放程式播放一次收集本身的樣式。 您可以先定義代表每個角色，才能在指定播放程式中指定唯一的位元旗標`xim_player_roles_and_skill_configuration`結構。

下列範例會設定本機 xim_player 物件，其指標是 'localPlayer' 的應用程式專屬 MYROLEBITFLAG_HEALER uint8_t 角色值：

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.roles = MYROLEBITFLAG_HEALER;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

完成此動作，將會提供所有參與者都`xim_player_roles_and_skill_configuration_changed_state_change`指出這`xim_player`已變更其每個播放程式的角色設定。 新的值可以藉由呼叫擷取`xim_player::roles_and_skill_configuration()`。

全域`xim_matchmaking_configuration`若要指定結構`xim::move_to_network_using_matchmaking()`應該擁有所有必要的角色旗標結合使用位元 OR，而值為 true require_player_matchmaking_configuration 欄位。

下列範例會填入無小組戰鬥中找到的 3 個播放程式總數的配對設定。 此外，此範例中使用的應用程式定義的常數，它是型別、 vector<pair<uint64_t 且名為 MYGAMEMODE_COOPERATIVE，表示要關閉的篩選的遊戲模式。 此外，設定要在每個播放程式的角色和技能組態，其中至少一名玩家，可滿足每個應用程式專屬 uint8_t 角色是位元 OR 必須一起並放置在 設定 （MYROLEBITFLAG_HEALER、 MYROLEBITFLAG_近戰和 MYROLEBITFLAG_RANGE）：

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 3;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 3;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_COOPERATIVE;
matchmakingConfiguration.required_roles = MYROLEBITFLAG_HEALER | MYROLEBITFLAG_MELEE | MYROLEBITFLAG_RANGE;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

此結構會提供給`xim::move_to_network_using_matchmaking()`，移動作業會啟動上面所述。

使用每個播放程式角色的配對也可以結合配對使用者每個播放程式的技能。 如果只有一個想要使用，請為其他指定值為 0。 這是因為它們具有所有宣告的玩家`xim_player_roles_and_skill_configuration`技能值為 0 會一律彼此相符，而且如果所有位元都是零`xim_matchmaking_configuration`required_roles 欄位，然後才能符合所不需的任何角色位元。

一次`xim::move_to_network_using_matchmaking()`或其他任何 XIM 網路移動作業完成後，所有的玩家`xim_player_roles_and_skill_configuration`結構將會自動清除為 null 指標 (及隨附的`xim_player_roles_and_skill_configuration_changed_state_change`通知)。 如果您打算將移至另一個使用需要每個播放器組態的配對的 XIM 網路，您必須呼叫`xim_player::xim_local::set_roles_and_skill_configuration()`再次使用新的結構指標，包含最新的資訊。

## <a name="how-xim-works-with-player-teams"></a>XIM 與播放程式團隊的運作方式

多人遊戲通常牽涉到組織到相反小組的播放程式。 XIM 可讓您輕鬆地指派小組時藉由設定配對`xim_team_configuration`。 下列範例會起始移動，使用配對設定為找出總共 8 播放器，將兩個相等的小組，共 4 （雖然如果找不到 4，1-3 玩家也是可接受的）、 使用應用程式特定的遊戲模式常數、 vector<pair<uint64_t 值所定義只會使用其他播放器來指定相同的值，並將所有的社交加入播放程式，從目前的 XIM 網路比對的 MYGAMEMODE_CAPTURETHEFLAG:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 2;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 1;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 4;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_CAPTURETHEFLAG;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

這類 XIM 網路移動作業完成時，播放程式將會指派小組索引值為 1 到 {n} {n} 小組要求相對應。 任何特定小組的索引值，則為 true 的意義是由應用程式。 玩家的小組索引值透過擷取`xim_player::team_index()`。 使用時`xim_team_configuration`使用兩個或多個小組，播放程式會永遠不會指派小組索引值為零的呼叫所`xim::move_to_network_using_matchmaking()`。 這是相較之下的播放程式會新增至 XIM 網路組態或移動作業的類型 (例如透過接受邀請所產生的通訊協定啟動)，誰會一律具有零索引，小組。 它可能會有幫助視為特殊的 「 未指派 」 小組的小組索引 0。

下列範例會擷取小組索引指標是在 'ximPlayer' 變數 xim_player 物件：

```cpp
uint8_t playerTeamIndex = ximPlayer->team_index();
```

（不以負玩家行為值得一提，降低機會） 的慣用的使用者經驗，Xbox Live 配對服務永遠不會分割球員要移動到不同的小組 XIM 網路。

一開始指派配對的小組索引值是僅供建議和應用程式可以將它變更為本機的播放程式在任何時候使用`xim_player::xim_local::set_team_index()`。 這也稱為完全不使用配對的 XIM 網路中。 下列範例會設定 player 指標 'localPlayer'，有新的小組索引值的其中一個：

```cpp
localPlayer->local()->set_team_index(1);
```

所有裝置都能收到都通知，播放程式有新的小組索引值生效時它們會提供`xim_player_team_index_changed_state_change`該播放程式。 您可以呼叫`xim_player::xim_local::set_team_index()`在任何時間。

配對會評估分開小組的每個玩家的必要的角色。 因此不建議小組和必要的角色使用作為同時配對設定準則，因為小組會藉由播放程式次數、 平衡不是由已完成的播放程式的角色。

## <a name="working-with-chat"></a>使用對談

語音和文字的聊天通訊 XIM 網路中的播放程式之間，會自動啟用。 XIM 會處理您所有的語音耳機與麥克風硬體互動。 您的應用程式不需要執行許多作業，來交談，但它的確有關於文字交談的一項需求： 支援輸入和顯示。 需要文字輸入，因為即使在平台或遊戲的內容類型，在過去一直沒有普遍使用的實體鍵盤，播放程式可以設定要使用文字轉換語音的輔助技術的系統。 同樣地，文字顯示需要，因為播放程式可以設定要使用語音轉換文字的系統。

可以在本機的播放程式偵測這些喜好設定，藉由呼叫`xim_player::xim_local::chat_text_to_speech_conversion_preference_enabled()`和`xim_player::xim_local::chat_speech_to_text_conversion_preference_enabled()`，而且您可能想要有條件地啟用文字機制。 不過，我們建議您考慮進行輸入和顯示一律可用之選項的文字。

 `Windows::Xbox::UI::Accessability` 是的 Xbox One 類別專為著重在語音轉換文字的輔助技術提供簡單的遊戲中的文字對談的轉譯。

提供的實際或虛擬鍵盤的文字輸入之後，將字串傳遞至`xim_player::xim_local::send_chat_text()`方法。 下列程式碼示範從本機傳送範例硬式編碼字串`xim_player`指向的變數 'localPlayer' 物件：

```cpp
localPlayer->local()->send_chat_text(L"Example chat text");
```

此聊天文字會傳遞到所有播放程式可以從原始的本機 player 接收交談通訊 XIM 網路中。 它可能會合成語音的音訊，並可能會提供它做`xim_chat_text_received_state_change`。

您的應用程式應該建立一份已接收的任何文字字串，和身分識別資料來源的播放程式一起顯示的適當數量的時間 （或在可捲動視窗中）。

另外還有一些關於交談的最佳作法。 建議是，任何位置播放程式會顯示，特別是在一份玩家代號，例如計分板中，您也顯示為使用者的意見反應的靜音/說話的圖示。 這是藉由呼叫`xim_player::chat_indicator()`擷取`xim_player_chat_indicator`表示目前、 即時狀態的交談，播放程式。 下列範例示範如何擷取的指標值`xim_player`物件指向的變數 'ximPlayer'，若要判斷特定圖示常值，指派給 'iconToShow' 變數：

```cpp
switch (ximPlayer->chat_indicator())
{
   case xim_player_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

所報告的值`xim_player::chat_indicator()`預期會經常變更播放程式開始和停止談話，例如。 它被設計來支援輪詢每個 UI 畫面格結果的應用程式。

> [!NOTE]
> 如果本機使用者沒有足夠的通訊權限，因為他們的裝置設定，而`xim_player::chat_indicator()`會傳回`xim_player_chat_indicator::platform_restricted`。 在預期情況下，以符合需求的平台是應用程式顯示，指出語音聊天或傳訊和指出問題的使用者訊息的平台限制的遙控器。 我們建議的一個範例訊息如下：「 很抱歉，您不是允許聊天。 」

## <a name="muting-players"></a>靜音播放程式

另一個的最佳做法是以支援靜音播放程式。 XIM 會自動處理系統靜音由使用者透過播放程式卡，但應用程式應支援遊戲特有暫時性靜音，可透過遊戲 UI 內執行`xim_player::set_chat_muted()`方法。 下列範例會先靜音遠端`xim_player`物件指向的變數 'remotePlayer' 以便聽過沒有語音交談和從它收到沒有文字對談：

```cpp
remotePlayer->set_chat_muted(true);
```

靜音會立即生效，而且沒有任何`xim_state_change`與其相關聯。 它可以藉由呼叫復原`xim_player::set_chat_muted()`再次具有 false 的值。 下列範例 unmutes 遠端`xim_player`指向的變數 'remotePlayer' 物件：

```cpp
remotePlayer->set_chat_muted(false);
```

Mutes 仍然有效的只要`xim_player`存在，包括將移到新的 XIM 網路與播放器時。 它不會保存如果玩家要離開並重新加入相同的使用者 (作為新`xim_player`執行個體)。

播放程式通常會啟動 unmuted 的狀態。 如果您的應用程式想要靜音的狀態，原因遊戲啟動播放程式，它可以呼叫`xim_player::set_chat_muted()`上`xim_player`再完成處理相關聯的物件`xim_player_joined_state_change`，而且 XIM 會保證會有任何一段時間，語音音訊讓玩家可以聽到。

遠端的播放程式加入 XIM 網路時，就會發生根據播放程式信譽自動靜音檢查。 如果播放程式信譽不佳的旗標，播放程式自動靜音。 靜音只會影響本機狀態，因此如果播放器移過網路會保存。 執行一次自動評價靜音檢查且未重新評估一次只要`xim_player`會維持有效狀態。

## <a name="configuring-chat-targets-using-player-teams"></a>設定使用播放程式團隊的交談目標

中所述[如何 XIM 搭配 player 小組](#how-xim-works-with-player-teams)本文件中，則為 true 的任何特定小組的索引值意義的區段是由應用程式。 XIM 不將它們解譯除了相對於交談的目標組態的相等比較。 如果交談目標設定報告`xim::chat_targets()`目前`xim_chat_targets::same_team_index_only`，然後如果兩個具有相同的值所報告，任何給定的播放程式只會交換與另一個交談通訊`xim_player::team_index()`(和隱私權原則也允許 /)。

保守的且支援競爭的情況下，新建立的 XIM 網路會自動設定預設為`xim_chat_targets::same_team_index_only`。 不過，透過擊敗的對手，其他小組可能需要的例如，在後續的遊戲 「 入口 」。 您可以指示以允許所有人都能向其他人 （其中隱私權和原則允許） 藉由呼叫 XIM `xim::set_chat_targets()`。 下列範例會開始設定 XIM 網路中使用的所有參與者`xim_chat_targets::all_players`值：

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

所有參與者會收到都通知您新的目標設定為作用中時它們會提供`xim_chat_targets_changed_state_change`。

如先前所述，大多數 XIM 網路移動類型將會一開始指派所有播放程式小組索引值為零。 這表示的組態`xim_chat_targets::same_team_index_only`可能無法區別`xim_chat_targets::all_players`預設。 不過，播放程式將移至使用配對 XIM 網路會有不同小組索引值，如果配對設定`xim_team_matchmaking_mode`宣告兩個或多個小組的值。 您也可以呼叫`xim_player::xim_local::set_team_index()`次如上所示。 如果您的應用程式使用非零小組透過其中一種方法的索引值，別忘了管理適當地設定目前的交談目標。

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>自動背景填滿的 player 位置 （「 回填 」 配對）

不同群組的播放程式呼叫`xim::move_to_network_using_matchmaking()`同時提供 Xbox Live 配對服務快速地將它們組織到新的、 最佳 XIM 網路最大的彈性。 不過，某些遊戲案例想要保留特定 XIM 網路保持不變，並只 matchmake 其他播放程式只以填滿空的玩家資料。 XIM 支援設定操作在幕後自動填入模式或 「 回填 」，藉由呼叫配對`xim::set_network_configuration()`具有`xim_network_configuration`具有`xim_allowed_player_joins::matchmade`旗標設定在其`xim_network_configuration::allowed_player_joins`屬性。

下列範例會回填配對，以嘗試尋找沒有小組戰鬥 8 的播放程式總數 （雖然如果找不到 8，2-7 玩家也是可接受的），使用應用程式特定的遊戲模式常數、 vector<pair<uint64_t 值 MYGAMEMODE_ 所定義只會與其他玩家指定相同的值符合的生死：

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::matchmade;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 2;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

這可讓現有的 XIM 網路提供給呼叫的裝置`xim::move_to_network_using_matchmaking()`以正常方式。 這些裝置，請參閱變更任何行為。 回填 XIM 網路參與者不會移動，但會提供`xim_network_configuration_changed_state_change`表示回填開啟，以及多個`xim_matchmaking_progress_updated_state_change`適用時的通知。 任何 matchmade 播放程式將會新增至 XIM 網路時使用一般`xim_player_joined_state_change`。

根據預設，回填配對仍在進行中無限期地，雖然它不會嘗試新增玩家，如果 XIM 網路已經玩家 xim_team_configuration 設定所指定的最大數目。 不允許 matchmade 設定 xim_allowed_player_joins 回填工作可以停用。 下列範例會回填工作，藉由清除 xim_allowed_player_joins::matchmade 旗標，同時保留所有其他現有的旗標和網路組態設定。

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins &= ~xim_allowed_player_joins::matchmade;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

相對應`xim_network_configuration_changed_state_change`將會提供所有裝置，並且此非同步程序完成後，最終`xim_matchmaking_progress_updated_state_change`將會得到`xim_matchmaking_status::none`表示沒有進一步的 matchmade 玩家會加入 XIM 網路。

啟用與回填配對時`xim_team_configuration`設定會宣告兩個或多個小組，所有現有的播放程式必須有有效的 team 索引是介於 1 到小組的數目。 這包括呼叫的球員`xim_player::xim_local::set_team_index()`若要指定自訂值或已加入使用邀請或透過其他社交表示人員 (例如，若要呼叫`xim::move_to_network_using_joinable_xbox_user_id`) 和已加入預設小組的索引值為 0。 如果任何播放程式沒有有效的 team 索引，則配對程序將會暫停，而且會提供所有參與者都`xim_matchmaking_progress_updated_state_change`與`xim_matchmaking_status::waiting_for_player_team_index`值。 一旦有提供所有的玩家，或將其更正它們的小組索引值與`xim_player::xim_local::set_team_index()`，回填配對將會繼續。 中可以找到詳細資訊[如何 XIM 搭配 player 小組](#how-xim-works-with-player-teams)這份文件的區段。

同樣地，啟用與回填配對時`xim_network_configuration`結構和`require_player_roles_and_skill_configuration`欄位設為 true 的角色或技術，則所有的玩家必須指定非 null 每位玩家配對設定。 如果未指定任何播放程式，則配對程序將會暫停，而且會提供所有參與者都`xim_matchmaking_progress_updated_state_change`與`xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration`值。 一旦所有播放程式提供其`xim_player_roles_and_skill_configuration`結構回填配對將會繼續。 中可以找到詳細資訊[配對使用每個播放程式技能](#matchmaking-using-per-player-skill)並[配對使用每個玩家角色](#matchmaking-using-per-player-role)本文件的章節。

## <a name="querying-joinable-networks"></a>查詢可聯結的網路

適合用來連接玩家一起快速配對時，有時最好是讓玩家能探索使用自訂的搜尋準則的可聯結網路，並選取他們想要加入的網路。 遊戲工作階段可能會有大量的可設定的遊戲規則和播放程式喜好設定時，這可以是特別有用。 若要這樣做，現有的網路必須先設定為可查詢藉由啟用`xim_allowed_player_joins::queried`joinability 和設定的其他人可以透過呼叫網路外部使用的網路資訊`xim::set_network_configuration()`。

下列範例會啟用`xim_allowed_player_joins::queried`joinability，設定網路以允許 1-8 播放器總計值 GAME_MODE_BRAWL，所定義的常數、 vector<pair<uint64_t 應用程式特定的遊戲模式，1 小組在一起的小組設定的組態說明「 cat 和為伍的 boxing 比對 」，應用程式專屬對應索引常數 uint32_t MAP_KITCHEN 值所定義，並且包含 spectatorallowed 標記"chatrequired 」 「 簡單 」，「 」:

```cpp
PCWSTR tags[] = { L"chatrequired", L"easy", L"spectatorallowed" };
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::queried;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 1;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = GAME_MODE_BRAWL;
networkConfiguration.description = L"cat and sheep's boxing match";
networkConfiguration.map_index = MAP_KITCHEN;
networkConfiguration.tag_count = _countof(tags);
networkConfiguration.tags = tags;

xim::set_network_configuration(&networkConfiguration);
```

在網路外部的其他播放程式可以接著找到網路藉由呼叫`xim::start_joinable_network_query()`與一組與網路資訊，在上一個相符的篩選`xim::set_network_configuration()`呼叫。 下列範例會將可聯結網路查詢開始遊戲模式篩選選項，只會查詢網路使用 GAME_MODE_BRAWL 值所定義的應用程式特定遊戲模式：

```cpp
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;

xim::start_joinable_network_query(queryFilters);
```

以下是使用標記篩選選項查詢具有標記為 「 簡單 」 的網路和 「 spectatorallowed"，其可供查詢的公用組態中的另一個範例：

```cpp
PCWSTR tagFilters[] = { L"easy", L"spectatorallowed" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

也可以結合不同的篩選選項。 下列範例會使用遊戲模式篩選選項和標記篩選選項來啟動網路都有的應用程式特定的遊戲模式常數 GAME_MODE_BRAWL 和標籤 「 簡單 」 的查詢：

```cpp
PCWSTR tagFilters[] = { L"easy" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

如果查詢作業成功時，應用程式將會收到`xim_start_joinable_network_query_completed_state_change`從中應用程式可以擷取一份可聯結的網路。 應用程式會持續地也接收`xim_joinable_network_query_updated_state_change`其他可加入網路，或剛好的可聯結網路傳回的清單，直到以手動或自動方式停止的任何變更。 可以手動停止進行中查詢，藉由呼叫`xim::stop_joinable_network_query()`。 它將會自動停止時呼叫`xim::start_joinable_network_query()`來啟動新的查詢。

應用程式可以嘗試在可聯結的網路清單中加入網路，藉由呼叫`xim::move_to_network_using_joinable_network_information()`。 下列範例假設您想要參加`xim_joinable_network_information`指標 selectedNetwork' ' 是不受保護的密碼，因此我們會將 nullptr 傳遞給第二個參數） 所指：

```cpp
xim::move_to_network_using_joinable_network_information(selectedNetwork, nullptr);
```

藉由呼叫時啟用網路會宣告兩個或多個小組 xim_team_configuration 查詢，加入玩家`xim::move_to_network_using_joinable_network_information()`會有預設小組的索引值為 0。

> [!NOTE]
> 如果應用程式已指定多個本機使用者，並加入具有較少的空間比本機的使用者數目的網路，聯結可以仍然會成功。 但只允許本機使用者的數目可能會加入網路。