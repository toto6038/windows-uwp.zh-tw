---
title: 使用遊戲聊天 2
description: 了解如何使用 Xbox Live 遊戲聊天 2 加入您的遊戲中的語音通訊。
ms.date: 03/20/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 遊戲聊天 2、 遊戲的對談、 語音通訊
ms.localizationpriority: medium
ms.openlocfilehash: 43fbd7cec037df735686aa60bc6cd57217875e33
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639253"
---
# <a name="using-game-chat-2-c"></a>使用遊戲聊天 2 （c + +）

這是使用遊戲聊天 2 的 c + + API 的簡短逐步解說。 遊戲開發人員想要存取受遊戲對談的 2 到C#應該會看到[使用遊戲聊天 2 WinRT 投影](using-game-chat-2-winrt.md)。

1. [必要條件](#prerequisites)
2. [初始化](#initialization)
3. [設定使用者](#configuring-users)
4. [處理資料框架](#processing-data-frames)
5. [處理狀態變更](#processing-state-changes)
6. [文字對談](#text-chat)
7. [協助工具](#accessibility)
8. [UI](#ui)
9. [靜音](#muting)
10. [不正確的信譽自動靜音](#bad-reputation-auto-mute)
11. [權限及隱私權](#privilege-and-privacy)
12. [清除](#cleanup)
13. [失敗的模型](#failure-model)
14. [如何設定受歡迎的案例](#how-to-configure-popular-scenarios)

## <a name="prerequisites"></a>必要條件

開始使用遊戲聊天 2 撰寫程式碼之前，您必須設定您的應用程式 AppXManifest 宣告 「 麥克風 」 裝置功能。 AppXManifest 功能詳述於個別的平台文件中; 區段中的更多詳細資料下列程式碼片段會顯示 「 麥克風 」 裝置功能節點應該存在於 [封裝/功能] 節點下，否則會封鎖對談：

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

編譯遊戲聊天 2 需要包含主要 GameChat2.h 標頭。 若要正確連結，您的專案也必須包含 GameChat2Impl.h 中至少一個 （因為這些虛設常式的函式實作都是小型且容易編譯器產生為 「 內嵌 」，常見的先行編譯標頭建議選項） 的編譯單位。

遊戲聊天 2 介面不需要的專案，選擇 編譯以 C + + /CX 與傳統的 c + +;它可以搭配使用。 實作也不會擲回例外狀況做為報告，所以您可以使用它輕鬆地從無例外狀況的專案如果慣用的非嚴重錯誤。 實作，不過，擲回例外狀況做為嚴重的錯誤報告 (請參閱[失敗模型](#failure)如需詳細資訊)。

## <a name="initialization"></a>初始化

您開始進行互動的程式庫初始化遊戲聊天 2 單一執行個體，其參數會套用至單一的初始設定的存留期。

```cpp
chat_manager::singleton_instance().initialize(...);
```

## <a name="configuring-users"></a>設定使用者

一旦初始化執行個體時，必須以遊戲聊天 2 執行個體加入本機使用者。 在此範例中，使用者 A 將代表的本機使用者。

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(<user_a_xuid>);
```

您也必須將遠端使用者和識別項，將會用來代表遠端 「 端點 」 上的使用者。 「 端點 」 是在遠端裝置上執行的應用程式的執行個體。 在此範例中，使用者 B 會在結束點的 X、 使用者 C 和 D 會在端點 y 端點 X 任意指派識別碼"1"，並且端點 Y 任意指派識別碼"2"。 您通知這些呼叫的遠端使用者的遊戲聊天 2:

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(<user_b_xuid>, 1);
chat_user* chatUserC = chat_manager::singleton_instance().add_remote_user(<user_c_xuid>, 2);
chat_user* chatUserD = chat_manager::singleton_instance().add_remote_user(<user_d_xuid>, 2);
```

現在您可以設定每個遠端使用者與本機使用者之間的通訊關聯性。 在此範例中，假設使用者 A 和 B 使用者位於相同的小組，並允許雙向通訊。 `c_communicationRelationshipSendAndReceiveAll` 代表雙向通訊 GameChat2.h 中定義的常數。 使用者 A 的關聯性設為使用者 B，其中：

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

現在假設使用者 C 和 D 會 'spectators'，而且應該可以接聽使用者的但未說明。 `c_communicationRelationshipSendAll` 來代表此單向通訊的 GameChat2.h 中定義的常數。 設定與關聯性：

```cpp
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

如果在任何時間點有已加入至單一執行個體，但還沒有已設定為任何本機使用者-與通訊的遠端使用者，就是沒問題 ！ 這可能會在使用者決定小組，或可以任意變更讀出通道的情況下發生。 遊戲的對談 2 只會快取已新增執行個體，就很有用，通知遊戲聊天 2 的所有可能的使用者，即使它們不能說出任何特定時間點的本機使用者時間的使用者資訊 （例如隱私權的關聯性和評價）。

最後，假設使用者 D 離開遊戲，且應從本機的遊戲的對談 2 執行個體中移除。 可以使用下列呼叫來完成：

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

呼叫`chat_manager::remove_user()`可能會使使用者物件。 如果您使用[即時音訊操作](real-time-audio-manipulation.md)，請參閱[聊天使用者存留期](real-time-audio-manipulation.md#chat-user-lifetimes)文件的進一步資訊。 否則，使用者物件會立即失效時`chat_manager::remove_user()`呼叫。 使用者可以移除時的細微限制會詳細說明[處理狀態變更](#state)。

## <a name="processing-data-frames"></a>處理資料框架

遊戲的對談 2 並沒有自己的傳輸層級;這必須由應用程式提供。 此外掛程式由應用程式的規則，經常呼叫`chat_manager::start_processing_data_frames()`和`chat_manager::finish_processing_data_frames()`一組方法。 這些方法是遊戲聊天 2 如何提供應用程式傳出的資料。 它們可以快速地操作，使它們可以輪詢專用的網路執行緒中的常見問題。 這提供方便的地方，而不需擔心網路計時或複雜的多執行緒的回呼的不可預測性擷取所有已排入佇列的資料。

當`chat_manager::start_processing_data_frames()`呼叫時，所有已排入佇列的資料會報告中的陣列`game_chat_data_frame`結構的指標。 應用程式應逐一查看陣列、 檢查目標 [端點]，並使用應用程式的網路層的資料傳送至適當的遠端應用程式執行個體。 一旦完成所有`game_chat_data_frame`結構陣列應該傳遞回遊戲聊天 2 來釋放資源，藉由呼叫`chat_manager:finish_processing_data_frames()`。 例如：

```cpp
uint32_t dataFrameCount;
game_chat_data_frame_array dataFrames;
chat_manager::singleton_instance().start_processing_data_frames(&dataFrameCount, &dataFrames);
for (uint32_t dataFrameIndex = 0; dataFrameIndex < dataFrameCount; ++dataFrameIndex)
{
    game_chat_data_frame const* dataFrame = dataFrames[dataFrameIndex];
    HandleOutgoingDataFrame(
        dataFrame->packet_byte_count,
        dataFrame->packet_buffer,
        dataFrame->target_endpoint_identifier_count,
        dataFrame->target_endpoint_identifiers,
        dataFrame->transport_requirement
        );
}
chat_manager::singleton_instance().finish_processing_data_frames(dataFrames);
```

資料框架處理的次數越頻繁，音訊終端使用者可明顯的延遲會越低。 音訊被聯合成 40 毫秒資料框架;這是建議的輪詢間隔。

## <a name="processing-state-changes"></a>處理狀態變更

遊戲的對談 2 提供應用程式，例如接收的文字訊息，透過應用程式的一般、 經常呼叫來更新`chat_manager::start_processing_state_changes()`和`chat_manager::finish_processing_state_changes()`一組方法。 它們可以快速地操作，使它們可以在您的 UI 轉譯迴圈中呼叫每個圖形畫面格。 這提供方便的地方，而不需擔心網路計時或複雜的多執行緒的回呼的不可預測性擷取所有佇列中的變更。

當`chat_manager::start_processing_state_changes()`是呼叫，所有已排入佇列的更新中所報告的陣列`game_chat_state_change`結構的指標。 應用程式應該逐一查看陣列、 檢查其更特定類型的基底結構，請將轉換至對應的更詳細類型，然後再處理該更新為適當的基礎結構。 一旦完成所有`game_chat_state_change`目前可用的物件，該陣列應該傳遞回遊戲聊天 2 來釋放資源，藉由呼叫`chat_manager::finish_processing_state_changes()`。 例如：

```cpp
uint32_t stateChangeCount;
game_chat_state_change_array gameChatStateChanges;
chat_manager::singleton_instance().start_processing_state_changes(&stateChangeCount, &gameChatStateChanges);

for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; ++stateChangeIndex)
{
    switch (gameChatStateChanges[stateChangeIndex]->state_change_type)
    {
        case game_chat_state_change_type::text_chat_received:
        {
            HandleTextChatReceived(static_cast<const game_chat_text_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        case Xs::game_chat_2::game_chat_state_change_type::transcribed_chat_received:
        {
            HandleTranscribedChatReceived(static_cast<const Xs::game_chat_2::game_chat_transcribed_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_state_changes(gameChatStateChanges);
```

因為`chat_manager::remove_user()`立即失效的使用者物件相關聯的記憶體狀態的變更可能包含使用者物件的指標和`chat_manager::remove_user()`不必須處理的狀態變更時呼叫。

## <a name="text-chat"></a>文字對談

若要傳送的文字對談，請使用`chat_user::chat_user_local::send_chat_text()`。 例如：

```cpp
chatUserA->local()->send_chat_text(L"Hello");
```

遊戲的對談 2 會產生資料框架，其中包含此訊息;資料框架的目標端點將會是已設定為接收來自本機使用者的文字的使用者相關聯。 遠端端點所處理的資料時，會透過公開訊息`game_chat_text_chat_received_state_change`。 如同語音聊天權限及隱私權限制會遵守文字聊天。 如果使用者的一組已設定為允許文字聊天，但權限或隱私權限制不允許該通訊，文字訊息皆會予以捨棄。

支援文字聊天輸入和顯示是為了協助工具 (請參閱[協助工具](#access)如需詳細資訊)。

## <a name="accessibility"></a>協助工具

需要支援輸入的文字對談和顯示。 需要文字輸入，因為即使在平台或遊戲的內容類型，在過去一直沒有普遍使用的實體鍵盤上，使用者可以設定要使用文字轉換語音的輔助技術的系統。 同樣地，文字顯示需要，因為使用者可以設定要使用語音轉換文字的系統。 這些喜好設定偵測不到本機使用者藉由呼叫`chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()`和`chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()`方法分別，而且您可能想要有條件地啟用文字機制。

### <a name="text-to-speech"></a>文字轉換語音

當使用者擁有已啟用，文字轉換語音`chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()`會傳回 'true'。 偵測到此狀態時，應用程式必須提供文字輸入的方法。 一旦您擁有實際或虛擬鍵盤所提供的文字輸入，將字串傳遞至`chat_user::chat_user_local::synthesize_text_to_speech()`方法。 遊戲的對談 2 會偵測並合成根據字串和使用者的存取語音喜好設定的音訊資料。 例如：

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

合成的這項作業一部分的音訊會傳輸至已設定為接收音訊從這個本機使用者的所有使用者。 如果`chat_user::chat_user_local::synthesize_text_to_speech()`上並沒有文字轉換語音啟用的遊戲聊天 2 會不採取任何動作的使用者呼叫。

### <a name="speech-to-text"></a>語音轉換文字

當使用者語音轉換文字啟用，`chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()`會傳回 true。 偵測到此狀態時，應用程式必須準備好提供與轉譯的聊天訊息相關聯的 UI。 遊戲的對談 2 會自動轉譯每個遠端使用者的音訊，並將它透過公開`game_chat_transcribed_chat_received_state_change`。

> `Windows::Xbox::UI::Accessibility` 是的 Xbox One 類別專為著重在語音轉換文字的輔助技術提供簡單的遊戲中的文字對談的轉譯。

### <a name="speech-to-text-performance-considerations"></a>語音轉換文字效能考量

啟用語音轉換文字時，每個遠端裝置上的遊戲聊天 2 執行個體就會起始與語音服務端點的 web 通訊端連線。 每個遠端的遊戲聊天 2 用戶端會將上傳音訊語音服務端點，透過此 websocket;語音服務端點偶而會傳回文字記錄訊息到遠端裝置。 遠端裝置接著傳送文字記錄訊息 （亦即文字訊息） 到本機裝置，其中將轉譯的訊息傳送遊戲聊天 2 應用程式來呈現。

因此，語音轉換文字的主要效能成本會是網路使用量。 大部分的網路流量是音訊的編碼的上傳。 Websocket 上傳已經完成編碼遊戲聊天 2 「 正常 」 的語音聊天路徑; 中的音訊應用程式可以透過位元速率控制`chat_manager::set_audio_encoding_type_and_bitrate`。

## <a name="ui"></a>UI

建議是，任何位置播放程式會顯示，特別是在一份玩家代號，例如計分板中，您也顯示為使用者的意見反應的靜音/說話的圖示。 這是藉由呼叫`chat_user::chat_indicator()`擷取`game_chat_user_chat_indicator`表示目前、 即時狀態的交談，播放程式。 下列範例示範如何擷取的指標值`chat_user`物件指向的變數 'chatUserA'，若要判斷特定圖示常值，指派給 'iconToShow' 變數：

```cpp
switch (chatUserA->chat_indicator())
{
   case game_chat_user_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::local_microphone_muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

所報告的值`xim_player::chat_indicator()`預期會經常變更播放程式開始和停止談話，例如。 它被設計來支援輪詢每個 UI 畫面格結果的應用程式。

## <a name="muting"></a>靜音

`chat_user::chat_user_local::set_microphone_muted()`方法可用來切換的本機使用者的麥克風靜音的狀態。 當麥克風靜音時，將會不擷取任何從該麥克風的音訊。 如果使用者是在共用的裝置，例如 Kinect 靜音的狀態就會套用到所有使用者。

`chat_user::chat_user_local::microphone_muted()`方法可用來擷取本機使用者的麥克風靜音狀態。 這個方法只會反映在軟體，透過對呼叫的本機使用者的麥克風是否已靜音`chat_user::chat_user_local::set_microphone_muted()`; 這個方法不會反映硬體靜音，比方說，透過控制使用者的耳機上的按鈕。 沒有任何方法來擷取使用者的音訊裝置透過遊戲聊天 2 的硬體靜音狀態。

`chat_user::chat_user_local::set_remote_user_muted()`方法可用來切換靜音狀態相對於特定的本機使用者的遠端使用者。 當遠端使用者設為靜音時，本機使用者不會聽到任何聲音，或者收到來自遠端使用者的文字訊息。

## <a name="bad-reputation-auto-mute"></a>不正確的信譽自動靜音

一般而言，遠端使用者會開始 unmuted。 遊戲的對談 2 將啟動使用者靜音的狀態 （1） 的遠端使用者不是本機使用者的朋友，以及 （2） 遠端使用者的信譽不佳旗標。 當使用者這項作業，因為靜音`chat_user::chat_indicator()`會傳回`game_chat_user_chat_indicator::reputation_restricted`。 此狀態將會覆寫的第一個呼叫`chat_user::chat_user_local::set_remote_user_muted()`，其中包含目標使用者的遠端使用者。

## <a name="privilege-and-privacy"></a>權限及隱私權

之上通訊關聯性設定的遊戲，遊戲的對談 2 會強制執行權限及隱私權限制。 當使用者第一次新增; 遊戲的對談 2 會執行特殊權限及隱私權限制查閱使用者的`chat_user::chat_indicator()`一律會傳回`game_chat_user_chat_indicator::silent`直到這項作業完成為止。 如果使用者使用的通訊會受到保護隱私權或權限限制，使用者的`chat_user::chat_indicator()`會傳回`game_chat_user_chat_indicator::platform_restricted`。 平台的通訊限制適用於語音和文字的聊天;絕對不會執行個體，其中文字聊天封鎖平台限制，但語音聊天不是，反之亦然。

`chat_user::chat_user_local::get_effective_communication_relationship()` 可用來協助您辨別使用者無法進行通訊，因為不完整的權限及隱私權作業時。 它會傳回強制執行遊戲聊天 2 的表單中的通訊關聯性`game_chat_communication_relationship_flags`原因關聯性可能不等於的形式設定的關聯性`game_chat_communication_relationship_adjuster`。 例如，如果查閱作業仍在進行中`game_chat_communication_relationship_adjuster`會`game_chat_communication_relationship_adjuster::intializing`。 此方法應該用於開發和偵錯的情況;不應該用來影響 UI (請參閱[UI](#UI))。

## <a name="cleanup"></a>Cleanup

當應用程式不再需要透過遊戲聊天 2 的通訊時，您應該呼叫`chat_manager::cleanup()`。 這可讓遊戲聊天 2 回收所管理的連絡事宜配置的資源。

## <a name="failure-model"></a>失敗的模型

遊戲聊天 2 實作不做為報告，所以您可以使用它輕鬆地從無例外狀況的專案如果慣用的非嚴重錯誤擲回例外狀況。 遊戲的對談 2，，會擲回例外狀況，告知有嚴重的錯誤。 這些錯誤是誤用的 API 之前初始化執行個體，或存取使用者物件，從遊戲聊天 2 執行個體移除後,，到遊戲交談執行個體中新增使用者等的結果。 這些錯誤會攔截在開發初期，並且能夠更正的修改與遊戲聊天 2 互動所使用的模式。 這類錯誤發生時，提示，當做在造成錯誤的原因會列印到偵錯工具中，才會引發例外狀況。

## <a name="how-to-configure-popular-scenarios"></a>如何設定受歡迎的案例

### <a name="push-to-talk"></a>推播至演講

應該使用實作推播至 talk `chat_user::chat_user_local::set_microphone_muted()`。 呼叫`set_microphone_muted(false)`允許語音和`set_microphone_muted(true)`將其限制。 這個方法會提供遊戲聊天 2 的最低延遲回應。

### <a name="teams"></a>小組

假設使用者 A 和使用者 B 位於 Team 藍色，而 C 使用者和使用者 D 位於 Team Red。 每位使用者是在應用程式的唯一執行個體。

在使用者 A 的裝置：

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
chatUserA->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserA->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

使用者 B 在裝置上：

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipSendAndReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

在 使用者 C 的裝置：

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAndReceiveAll);
```

使用者 D 在裝置上：

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAndReceiveAll);
```

### <a name="broadcast"></a>廣播

假設使用者 A 是讓訂單的領導者，而只能接聽使用者 B、 C 和 D。 每個播放程式是唯一的裝置上。

在使用者 A 的裝置：

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

使用者 B 在裝置上：

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

在 使用者 C 的裝置：

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

使用者 D 在裝置上：

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
```
