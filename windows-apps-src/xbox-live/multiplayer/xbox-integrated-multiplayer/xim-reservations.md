---
title: XIM Out of band 保留項目
description: 描述如何使用 Xbox 整合式多人遊戲 (XIM) 做為專用的聊天解決方案透過頻外保留項目。
ms.assetid: 0ed26d19-defb-414d-a414-c4877bd0ed37
ms.date: 01/28/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 xbox 整合式多人遊戲、 xim、 聊天
ms.localizationpriority: medium
ms.openlocfilehash: 82b38b8d0d94e4cccf501a101faee0e28a6b43c0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660833"
---
# <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>透過頻外保留，使用 XIM 做為專用聊天解決方案

大部分的應用程式使用 XIM 來處理集結玩家的每個層面。 畢竟，如其名，「Xbox 整合式多人遊戲」著重於組合端點對端點支援常見多人遊戲案例所需的所有功能。 不過有些應用程式可實作自己使用專用的網際網路伺服器的多人遊戲解決方案也想建立可靠、 低延遲、 低成本的對等聊天 communication 的優點。 XIM 會辨識這項需求，並支援延伸模式，以利用 XIM 的簡化的對等通訊，來增強外部的播放程式的管理，adobe flash XIM API 之外。 而不需要移動到 XIM 網路透過社交方法或配對，播放程式可讓您將使用 「 保留 」，保證屬於特定使用者的預留位置會交換 」 超出訊號範圍 」，透過應用程式的外部的播放程式會合機制。

移動過程中，除了 XIM 網路管理使用頻外保留項目都是有效的任何其他 XIM 網路相同。 所有的通訊功能的運作方式相同。 不過，配對和社交探索 API 方法一定是停用管理使用頻外保留項目，因為它們會與應用程式本身的外部實作衝突的 XIM 網路。 您無法從這類 XIM 網路中，例如傳送邀請。

>XIM 已最佳化，提供簡單的端對端解決方案。 因此，並非所有複雜的拓撲或案例可能會超出的保留項目最適合。 如果您有疑問是否或如何運用 XIM 的通訊功能，請連絡您的 Microsoft 代表。

後續的主題描述如何運用 XIM 中的頻外保留項目。 由於自上一節中所述的 「 標準 」 XIM 使用量相對較少的差異，有些討論被縮寫。 熟悉[使用 XIM](using-xim.md)建議。


## <a name="moving-to-a-new-out-of-band-reservation-xim-network"></a>移至新的頻外保留 XIM 網路

若要開始使用頻外保留項目，其中一個您收集的參與者必須移至新的 XIM 網路，在此模式中建立。 裝置是由您決定哪一個參與的對等項目的選取範圍。 您可能已經有一個概念的遊戲主機或伺服器，也就是自然的選擇，來啟動處理程序，但這並非必要。 我們建議選擇報告的 [開啟] 網路存取類型，以便達到最快的連線設定時間的裝置。 請參閱`Windows::Networking::XboxLive`平台文件，如需詳細資訊。

移至 XIM 透過頻外保留項目管理的網路由初始化 XIM 和標準 XIM 使用量逐步解說中所示，宣告預定的本機 Xbox 使用者 Id，但而不是呼叫想`xim::move_to_new_network()`，呼叫`xim::move_to_network_using_out_of_band_reservation()`與保留 null 字串。 例如：

```cpp
 xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
 xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
 xim::singleton_instance().move_to_network_using_out_of_band_reservation(nullptr);
```

標準`xim_move_to_network_starting_state_change`， `xim_player_joined_state_change`，並`xim_move_to_network_succeeded_state_change`則會提供一段時間處理一般的狀態變更時`xim::start_processing_state_changes()`和`xim::finish_processing_state_changes()`迴圈。 例如：

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
             MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change *>(stateChange));
             break;
         }

         case xim_state_change_type::player_left:
         {
             MyHandlePlayerLeft(static_cast<const xim_player_left_state_change *>(stateChange));*
             break;
         }

         ...
     }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

之後的初始裝置已處理的狀態變更，但其玩家成功移動到 XIM 部署網路，它必須建立額外的使用者保留項目。

## <a name="adding-players-to-a-xim-network-managed-using-out-of-band-reservations"></a>使用頻外保留項目加入 XIM 網路中的播放程式管理

XIM 管理網路，一律使用頻外保留報告的值`xim_allowed_player_joins::out_of_band_reservation`從`xim::allowed_player_joins()`方法，它們關閉到所有播放程式，但不包括操作期間保留其 Xbox 使用者 Id 藉由呼叫`xim::create_out_of_band_reservation()`。 `xim::create_out_of_band_reservation()` 採用陣列的使用者，因此您可以建立您外部收集到的播放器這類保留項目，全部一次或一段時間。 此外，使用者已參與 XIM 網路的播放程式會被忽略，因此您也可以提供完整的取代集的其他 Xbox 使用者識別碼，或當差異變更時，兩者很方便。 下列範例假設您已經有您完整收集了一組 Xbox 使用者識別碼的字串指標陣列變數 'xboxUserIds' 與 'xboxUserIdCount' 數字的項目：

```cpp
 xim::singleton_instance().create_out_of_band_reservation(xboxUserIdCount, xboxUserIds);
```

這樣就會開始建立保留項目針對指定之 Xbox 使用者識別碼的非同步程序。 作業完成時，會提供 XIM`xim_create_out_of_band_reservation_completed_state_change`報告成功或失敗。 如果成功，保留項目字串將可供您的系統，以提供給作業提供一個 Xbox 使用者識別碼。 已成功建立保留項目字串有效期為只有一段時間。 這段時間內所傳回`xim_create_out_of_band_reservation_completed_state_change`。

使用有效的保留字串中，您用來蒐集外部 XIM 玩家的"超出訊號範圍 」 外部機制可用來發佈這些列舉的字串。 例如，如果您使用的多人遊戲工作階段目錄 (MPSD) Xbox Live 服務，您可以撰寫這個字串做為工作階段的文件中的自訂共用的工作階段屬性 (注意： 保留項目字串一律會包含僅提供有限的字元安全使用 json 而不需要逸出或 Base64 編碼）。

當其他使用者從共用的工作階段的文件屬性擷取其保留字串時，然後開始移至使用它做為參數的 XIM 網路`xim::move_to_network_using_out_of_band_reservation()`。 下列範例會假設保留項目字串擷取至名為 'reservationString' 的變數。

```cpp
xim::singleton_instance().move_to_network_using_out_of_band_reservation(reservationString);

```

移動作業以非同步方式執行如同針對指定的保留項目字串為 null 指標的初始裝置。 狀態變更`xim_move_to_network_starting_state_change`， `xim_player_joined_state_change`，和`xim_move_to_network_succeeded_state_change`移動所產生。 當移動成功時，將會加入本機和遠端的播放程式。 將提供 XIM 網路上的現有裝置`xim_player_joined_state_change`的這些新的播放程式。 到目前為止，在 （其中隱私權和原則允許） 這個 XIM 網路中的這些不同裝置上的播放程式之間會自動啟用語音和文字的聊天通訊。

因為無效的保留項目字串，應該移動作業失敗，會傳回 XIM`xim_network_exited_state_change`原因`xim_network_exit_reason::invalid_reservation`。 這可能是由於多種原因所造成：

1. 標題嘗試呼叫`xim::move_to_network_using_out_of_band_reservation()`之前先在遠端裝置上`xim_create_out_of_band_reservation_completed_state_change`傳回主應用程式在裝置上
1. 標題嘗試呼叫`xim::move_to_network_using_out_of_band_reservation()`保留過期之後，在遠端裝置上。
1. 標題嘗試呼叫`xim::move_to_network_using_out_of_band_reservation()`多次時只會呼叫在遠端裝置上`xim::create_out_of_band_reservation()`之後。

無論成功或失敗，使用它們的移動作業所耗用的保留。 因此，每個使用嘗試之後，主應用程式應該產生新的保留項目字串呼叫`xim::create_out_of_band_reservation()`一次。

## <a name="configuring-chat-targets-in-a-xim-network-managed-using-out-of-band-reservations"></a>設定 XIM 網路中的交談目標受管理的使用頻外保留項目

XIM 管理網路，使用頻外保留項目到傳統 XIM 網路聊天通訊方面的行為相同。 若要支援競爭情況下，新建立的 XIM 網路會自動設定為僅支援與成員相同的小組的其他玩家的對談也就是設定的值回報`xim::chat_targets()`預設是`xim_chat_targets::same_team_index_only`。 這可以變更為允許所有人都能向其他人 （其中隱私權和原則允許） 藉由呼叫`xim::set_chat_targets()`。 下列範例會先設定要使用的 XIM 網路中的每個人都`xim_chat_targets::all_players`值：

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

所有參與者都能收到都通知，設定作用中的新目標時它們會提供`xim_chat_targets_changed_state_change`。

使用頻外保留項目管理 XIM 網路中的每個播放程式一開始是使用相同的小組索引值設定，為零。 這表示的組態`xim_chat_targets::same_team_index_only`區別`xim_chat_targets::all_players`預設。 不過，呼叫在任何時候變更本機玩家的小組索引`xim_player::xim_local::set_team_index()`。 下列範例會設定 player 指標 'localPlayer'，有新的小組索引值的其中一個：

```cpp
 localPlayer->local()->set_team_index(1);
```

玩家有新的小組索引值實際上就提供 xim_player_team_index_changed_state_change 該播放程式時，會通知所有的裝置。 如果交談目標組態目前是 xim_chat_targets::same_team_index_only，具有相同的新小組索引的其他播放程式會開始期待語音及所提供的文字對談 （隱私權和原則允許），從變更的播放程式，反之亦然。 使用舊的 team 索引的播放程式將會停止交換這種交談通訊。 如果交談目標組態目前 xim_chat_targets::all_players，小組索引就會有不會影響誰可以與其對談。

## <a name="removing-players-from-a-xim-network-managed-using-out-of-band-reservations"></a>移除 XIM 網路中的播放程式使用外的頻外保留項目管理

您使用頻外保留項目，以便您可能需要移除 XIM 網路中的播放程式時，外部管理球員的名單。 常見方式是讓應用程式運用相同的遊戲主機用來建立 XIM 網路和後續的保留項目，原本要也管理播放程式移除，及可以呼叫`xim::kick_player()`。 這會從 XIM 網路中移除指定的播放程式，並讓相同裝置上的所有播放程式。 下列範例會假設應用程式判定其想要移除`xim_player`指向 'playerToRemove' 變數的物件：

```cpp
xim::singleton_instance().kick_player(playerToRemove);
```

適用的播放程式 （或播放程式） 將會提供任何所需`xim_player_left_state_change`為每個玩家和`xim_network_exited_state_change`指出，它們已經被踢網路。 或者，可以呼叫每個玩家`xim::leave_network()`表示在自己的相同效果時結束。

請注意 XIM 對等通訊可能會自行判斷隨時玩家已離開 XIM 網路，因為環境的問題。 您的應用程式應該準備好處理的 「 不明 」`xim_player_left_state_change`及調解 XIM 的狀態和您的外部的播放程式管理配置，以適合您特定的應用程式的方式之間的任何差異。

## <a name="cleaning-up-a-xim-network-managed-using-out-of-band-reservations"></a>使用頻外保留清除 XIM 網路管理

還沒有已被踢 XIM 任何玩家網路，而且想要傳回的狀態，如同只有 xim::initialize() 並`xim::set_intended_local_xbox_user_ids()`呼叫，便可以開始這麼做，因此使用`xim::leave_network()`方法：

```cpp
xim::singleton_instance().leave_network();
```

這樣就會開始以非同步方式中斷其他參與者。 這會導致遠端裝置，必須提供`xim_player_left_state_change`將提供的本機的玩家，並在本機裝置`xim_player_left_state_change`對於每個播放器，本機或遠端。 所有的非失誤性中斷連線完成時，最終`xim_network_exited_state_change`會提供。 應用程式接著可以呼叫`xim::cleanup()`釋放所有資源，並傳回未初始化的狀態：

```cpp
 xim::singleton_instance().cleanup();
```

叫用`xim::leave_network()`並等候`xim_network_exited_state_change`以結束 XIM 網路依正常程序一律強烈建議您當`xim_network_exited_state_change`已經尚未提供。 呼叫`xim::cleanup()`直接可能會造成通訊效能問題剩餘的參與者而它們會強制裝置只是 「 消失 」 的逾時訊息。
