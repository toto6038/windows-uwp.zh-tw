---
title: 遊戲的聊天 2 移轉
description: 了解如何移轉現有的對談的遊戲程式碼，以使用遊戲聊天 2。
ms.date: 05/02/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 遊戲聊天 2、 遊戲的對談、 語音通訊
ms.localizationpriority: medium
ms.openlocfilehash: e963210091694a07114f10d5a3dc531a353621df
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653583"
---
# <a name="migration-from-game-chat-to-game-chat-2"></a>從遊戲的聊天移轉至遊戲的聊天 2

本文件詳述遊戲聊天和遊戲聊天 2 以及如何從遊戲聊天移轉至遊戲聊天 2 之間的相似性。 因此，它是目前已對談的遊戲實作想要移轉至遊戲聊天 2 的標題。 如果您還沒有遊戲對談的實作，建議的起始點就[聊天 2 使用遊戲](using-game-chat-2.md)。 

## <a name="preface"></a>前置詞

原始的遊戲聊天 API 是 WinRT API 公開對談到的使用者和語音通道協助 Xbox Live 的遊戲中語音聊天案例的實作的概念。 建置遊戲聊天 API 之上聊天 API，而其本身是公開的聊天使用者與語音通道的概念時需要低層級管理音訊裝置的 WinRT API。 遊戲的聊天 2 是原始的遊戲中交談和交談 Api 的後續版本-它被設計為一個簡單的 API，以基本交談的情況下，例如團隊溝通，同時提供進階的交談的情況下，例如廣播樣式的 更多的彈性通訊和即時音訊的操作。 遊戲的聊天和遊戲的聊天 2 填滿的相同利基： 針對 Xbox Live 的整合已啟用，遊戲中語音聊天，雖然標題會針對傳輸資料封包的遊戲的聊天或遊戲的遠端執行個體，以及提供傳輸層，每個 Api 會提供便利的方法對談 2。

遊戲聊天 2 API 有許多優於原始遊戲交談和交談的 Api。 一些重點包括：
* 彈性使用者導向不限於 「 通道 」 模型的 API。
* 由於封包篩選的資料來源和資料接收器的頻寬功能改進。
* 2 個長時間執行的執行緒，應用程式可設定的親和性內部限制。
* 以 c + + 和 WinRT 形式提供的 API。
* 簡化的耗用量模型與 「 運作 」 的模式。
* 用來重新導向透過自訂配置器的遊戲聊天 2 配置的記憶體攔截程序。
* 相同的 UWP + 獨佔資源應用程式 （紀元） 標頭，以更方便的跨平台開發體驗。

本文件詳述遊戲聊天和遊戲聊天 2 以及如何從遊戲聊天移轉至遊戲聊天 2 c + + API 之間的相似性。 如果您想要從遊戲聊天移轉至遊戲聊天 2 WinRT API，建議您閱讀本文以了解如何將遊戲聊天概念對應至遊戲聊天 2，，然後看到[使用遊戲聊天 2 WinRT 投影](using-game-chat-2-winrt.md)的WinRT 的特定模式。 原始遊戲談話，討論這份文件的範例程式碼會使用 C + + /CX。

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

遊戲聊天 2 介面不需要的專案，選擇 編譯以 C + + /CX 與傳統的 c + +;它可以搭配使用。 實作也不會擲回例外狀況做為報告，所以您可以使用它輕鬆地從無例外狀況的專案如果慣用的非嚴重錯誤。 實作，不過，擲回例外狀況做為嚴重的錯誤報告 (請參閱[失敗模型](#failure-model-and-debugging)如需詳細資訊)。

## <a name="initialization"></a>初始化

### <a name="game-chat"></a>遊戲的對談

與原始的遊戲聊天互動透過`ChatManager`類別。 下列範例示範如何建構`ChatManager`執行個體使用預設參數：

```cpp
auto chatManager = ref new ChatManager();
```

### <a name="game-chat-2"></a>遊戲交談 2

遊戲聊天 2 的所有互動都會都透過遊戲聊天 2 的`chat_manager`singleton。 發生任何有意義的互動的程式庫之前，必須先初始化 singleton。 單一要求，在初始化階段; 指定的並行的本機和遠端交談的使用者數目上限這是因為遊戲聊天 2 會預期的使用者數目成比例的記憶體預先配置。 下列範例示範如何初始化的單一執行個體時的並行的本機和遠端交談的使用者數目上限會是四個：

```cpp
chat_manager::singleton_instance().initialize(4);
```

有幾個可以初始化，在指定的選擇性參數，例如預設呈現遠端交談的使用者與遊戲聊天 2 是否應該執行自動語音轉換文字轉換的資料量。 如需這些參數的詳細資訊，請參閱文件`chat_manager::initialize()`。

## <a name="configuring-users"></a>設定使用者

### <a name="game-chat"></a>遊戲的對談

將本機使用者新增至原始的遊戲聊天 API 透過`ChatManager::AddLocalUserToChatChannelAsync()`。 將使用者新增至多個交談通道需要多次呼叫，每個指定不同的通道。 下列範例示範如何將使用者與 xuid"myXuid 」 加入通道 0:

```cpp
auto asyncOperation = chatManager->AddLocalUserToChatChannelAsync(0, L"myXuid");
```

遠端使用者不會新增至執行個體直接。 當遠端裝置上會出現在標題的網路時，標題就會建立物件，代表遠端裝置，並通知透過新裝置的對談的遊戲`ChatManager::HandleNewRemoteConsole()`。 標題也必須提供的比較物件代表遊戲交談的遠端裝置，藉由實作一種方法`CompareUniqueConsoleIdentifiersHandler`。 下列範例示範如何為遊戲對談，提供這個委派假設`Platform::String`物件用來代表遠端裝置和新的裝置，以字串表示`L"1"`已加入項目的網路：

```cpp
auto token = chatManager->OnCompareUniqueConsoleIdentifiers +=
    ref new CompareUniqueConsoleIdentifiersHandler(
        [this](Platform::Object^ obj1, Platform::Object^ obj2)
    {
        return (static_cast<Platform::String^>(obj1)->Equals(static_cast<Platform::String^>(obj2)));
    });

...

Platform::String^ newDeviceIdentifier = ref new Platform::String(L"1");
chatManager->HandleNewRemoteConsole(newDeviceIdentifier);
```

一旦遊戲交談有此裝置的通知，它會產生包含任何本機使用者的相關資訊的封包，並提供給項目的這些封包傳輸到遠端裝置上的遊戲交談執行個體。 同樣地，在遠端裝置上的遊戲交談執行個體將會產生包含該標題將會傳送至遊戲交談的本機執行個體的遠端裝置上使用者的相關資訊的封包。 當本機執行個體收到包含新的遠端使用者的相關資訊的封包時，新的遠端使用者會新增至本機執行個體的使用者清單，可透過`ChatManager::GetUsers()`。

從遊戲交談執行個體移除使用者會透過類似呼叫`ChatManager::RemoveLocalUserFromChatChannelAsync()`和 `ChatManager::RemoveRemoteConsoleAsync()`

### <a name="game-chat-2"></a>遊戲交談 2

將本機使用者新增到遊戲聊天 2 是以同步方式透過`chat_manager::add_local_user()`。 在此範例中，使用者 A 將代表 Xbox 使用者識別碼的本機使用者`L"myLocalXboxUserId"`:

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(L"myLocalXboxUserId");
```

請注意，使用者不被新增至特定的通道-遊戲聊天 2 而不是通道使用的 「 通訊關聯性 」 的概念來管理使用者是否可以對說話及了解彼此。 設定通訊關聯性的方法是在稍後解決這一節。

類似於本機的使用者，遠端使用者會新增同步至本機的遊戲聊天 2 的執行個體。 遠端使用者必須同時將用來代表遠端裝置的識別項相關聯。 遊戲的對談 2 是指的 「 端點 」 為遠端裝置上執行的應用程式執行個體。 在此範例中，使用者 B 會是 Xbox 使用者識別碼的使用者`L"remoteXboxUserId"`整數所表示的端點上`1`。

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(L"remoteXboxUserId", 1);
```

一旦使用者已新增至執行個體，您應該設定 「 通訊之間的關聯性 」 每個遠端使用者和每個本機使用者。 在此範例中，假設使用者 A 和 B 使用者位於相同的小組，並允許雙向通訊。 `c_communicationRelationshiSendAndReceiveAll` 代表雙向通訊 GameChat2.h 中定義的常數。 關聯性可以使用設定：

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

最後，假設使用者 B 離開遊戲，且應從遊戲交談的本機執行個體中移除。 會使用下列呼叫以同步方式執行：

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

請參閱[使用遊戲聊天 2-設定使用者](using-game-chat-2.md#configuring-users)如需詳細範例，或是參照為個別的 API 方法，如需詳細資訊。

## <a name="processing-data"></a>處理資料

### <a name="game-chat"></a>遊戲的對談

遊戲的對談並沒有自己的傳輸層級;這必須由應用程式提供。 輸出封包會由訂閱`OnOutgoingChatPacketReady`事件和檢查以判斷封包的目的地和傳輸需求的引數。 下列範例示範如何訂閱事件和轉送引數`HandleOutgoingPacket()`項目所實作的方法：

```cpp
auto token = chatManager->OnOutgoingChatPacketReady +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::ChatPacketEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::ChatPacketEventArgs^ args)
    {
        this->HandleOutgoingPacket(args);
    });
```

連入封包送出至遊戲聊天透過`ChatManager::ProcessingIncomingChatMessage()`。 必須提供未經處理的封包緩衝區與遠端裝置識別碼。 下列範例示範如何提交會儲存在本機的封包`packetBuffer`並儲存在本機變數中的遠端裝置識別碼`remoteIdentifier`。

```cpp
Platform::String^ remoteIdentifier = /* The identifier associated with the device that generated this packet */
Windows::Storage::Streams::IBuffer^ packetBuffer = /* The incoming chat packet */

chatManager->ProcessIncomingChatMessage(packetBuffer, remoteIdentifier);
```

### <a name="game-chat-2"></a>遊戲交談 2

同樣地，遊戲聊天 2 並沒有自己的傳輸層級;這必須由應用程式提供。 輸出封包都由應用程式的規則，經常呼叫`chat_manager::start_processing_data_frames()`和`chat_manager::finish_processing_data_frames()`一組方法。 這些方法是遊戲聊天 2 如何提供應用程式傳出的資料。 它們可以快速地操作，使它們可以輪詢專用的網路執行緒中的常見問題。 這提供方便的地方，而不需擔心網路計時或複雜的多執行緒的回呼的不可預測性擷取所有已排入佇列的資料。

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

內送資料提交到遊戲透過聊天 2 `chat_manager::processing_incoming_data()`。 必須提供的資料緩衝區和遠端端點的識別項。 下列範例示範如何提交本機變數中儲存的資料封包`dataFrame`並儲存在本機變數中的遠端端點識別碼`remoteEndpointIdentifier`:

```cpp
uin64_t remoteEndpointIdentier = /* The identifier associated with the endpoint that generated this packet */
uint32_t dataFrameSize = /* The size of the incoming data frame, in bytes */
uint8_t* dataFrame = /* A pointer to the buffer containing the incoming data */

chatManager::singleton_instance().process_incoming_data(remoteEndpointIdentifier, dataFrameSize, dataFrame);
```

## <a name="processing-events"></a>處理事件

### <a name="game-chat"></a>遊戲的對談

遊戲的交談使用事件處理模型時感興趣的項目，就會發生-例如接收文字訊息，或使用者的協助工具喜好設定的變更通知應用程式。 應用程式必須訂閱並實作針對每個感興趣的事件處理常式。 此範例示範如何訂閱`OnTextMessageReceived`事件，並轉送的引數`OnTextChatReceived()`或`OnTranscribedChatReceived()`應用程式所實作的方法：

```cpp
auto token = chatManager->OnTextMessageReceived +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^ args)
    {
        if (args->ChatTextMessageType != ChatTextMessageType::TranscribedSpeechMessage)
        {
            this->OnTextChatReceived(args);
        }
        else
        {
            this->OnTranscribedChatReceived(args);
        }
    });
```

### <a name="game-chat-2"></a>遊戲交談 2

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

### <a name="game-chat"></a>遊戲的對談

若要傳送的文字對談與遊戲中交談，`GameChatUser::GenerateTextMessage()`可用。 下列範例示範如何將聊天文字訊息傳送與所代表的本機聊天使用者`chatUser`變數：

```cpp
chatUser->GenerateTextMessage(L"Hello", false);
```

第二個的布林參數會控制文字轉換語音的轉換。 如需詳細資訊，請參閱 <<c0> [ 協助工具](#accessibility)。 遊戲的交談則會產生包含此訊息的交談封包。 遊戲交談的遠端執行個體將會透過簡訊收到通知`OnTextMessageReceived`事件。

### <a name="game-chat-2"></a>遊戲交談 2

若要傳送遊戲聊天 2 的文字對談，請使用`chat_user::chat_user_local::send_chat_text()`。 下列範例示範如何將聊天文字訊息傳送與所代表的本機聊天使用者`chatUser`變數：

```cpp
chatUser->local()->send_chat_text(L"Hello");
```

遊戲的對談 2 會產生資料框架，其中包含此訊息;資料框架的目標端點將會是已設定為接收來自本機使用者的文字的使用者相關聯。 遠端端點所處理的資料時，會透過公開訊息`game_chat_text_chat_received_state_change`。 如同語音聊天權限及隱私權限制會遵守文字聊天。 如果使用者的一組已設定為允許文字聊天，但權限或隱私權限制不允許該通訊，將會以無訊息方式卸除文字訊息。

支援文字聊天輸入和顯示是為了協助工具 (請參閱[協助工具](#accessibility)如需詳細資訊)。

## <a name="accessibility"></a>協助工具

需要遊戲聊天和遊戲聊天 2 支援輸入的文字對談和顯示。 需要文字輸入，因為即使在平台或遊戲的內容類型，在過去一直沒有普遍使用的實體鍵盤上，使用者可以設定要使用文字轉換語音的輔助技術的系統。 同樣地，文字顯示需要，因為使用者可以設定要使用語音轉換文字的系統。 遊戲聊天和遊戲聊天 2 提供偵測，並顧及使用者的協助工具設定; 的方法您可能想要有條件地啟用根據這些設定的文字機制。

### <a name="text-to-speech---game-chat"></a>文字轉換語音-遊戲的對談

當使用者擁有已啟用，文字轉換語音`GameChatUser::HasRequestedSynthesizedAudio()`會傳回 true。 偵測到此狀態時，`GameChatUser::GenerateTextMessage()`會另外產生插入至本機的使用者相關聯的音訊資料流的文字轉換語音音訊。 下列範例示範如何將聊天文字訊息傳送與所代表的本機使用者`chatUser`變數：

```cpp
chatUser->GenerateTextMessage(L"Hello", true);
```

第二個布林值參數用來讓應用程式的語音轉換文字喜好設定會覆寫-'true' 表示遊戲聊天應該文字轉換語音的音訊時產生使用者的文字轉換語音的喜好設定已啟用，而 'false' 表示的遊戲聊天訊息，應該永遠不會產生文字轉換語音的音訊。

### <a name="text-to-speech---game-chat-2"></a>文字轉換語音-遊戲的聊天 2

當使用者擁有已啟用，文字轉換語音`chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()`會傳回 'true'。 偵測到此狀態時，應用程式必須提供文字輸入的方法。 一旦您擁有實際或虛擬鍵盤所提供的文字輸入，將字串傳遞至`chat_user::chat_user_local::synthesize_text_to_speech()`方法。 遊戲的對談 2 會偵測並合成根據字串和使用者的存取語音喜好設定的音訊資料。 例如：

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

合成的這項作業一部分的音訊會傳輸至已設定為接收音訊從這個本機使用者的所有使用者。 如果`chat_user::chat_user_local::synthesize_text_to_speech()`上並沒有文字轉換語音啟用的遊戲聊天 2 會不採取任何動作的使用者呼叫。

### <a name="speech-to-text---game-chat"></a>語音轉換文字-遊戲的對談

當使用者語音轉換文字啟用，`GameChatUser::HasRequestSynthesizedAudio()`會傳回 'true'。 偵測到此狀態時，對談的遊戲會自動 ip-pbx 音訊的每個遠端使用者的音訊，並透過公開 （expose）`OnTextMessageReceived`事件。 當`OnTextMessageReceived`事件引發，因為訊息回條轉譯，而`TextMessageReceivedEventArgs`會指出訊息類型`ChatTextMessageType::TranscribedSpeechMessage`。

### <a name="speech-to-text---game-chat-2"></a>語音轉換文字-遊戲的聊天 2

當使用者語音轉換文字啟用，`chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()`會傳回 true。 偵測到此狀態時，應用程式必須準備好提供與轉譯的聊天訊息相關聯的 UI。 遊戲的對談 2 會自動轉譯每個遠端使用者的音訊，並將它透過公開`game_chat_transcribed_chat_received_state_change`。

> `Windows::Xbox::UI::Accessibility` 是的 Xbox One 類別專為著重在語音轉換文字的輔助技術提供簡單的遊戲中的文字對談的轉譯。

請參閱[使用遊戲聊天 2-語音轉換文字效能考量](using-game-chat-2.md#speech-to-text-performance-considerations)如需詳細資訊的語音轉換文字效能考量。

## <a name="ui"></a>UI

建議是，任何位置播放程式會顯示，特別是在一份玩家代號，例如計分板中，您也顯示為使用者的意見反應的靜音/說話的圖示。 遊戲的對談 2 導入了一個聯合的指標，提供更簡單方法來判斷適當的 UI 項目來顯示。

### <a name="game-chat"></a>遊戲的對談

A`GameChatUser`有三個屬性，決定適當的 UI 項目-時，通常會檢查`GameChatUser::TalkingMode`， `GameChatUser::IsMuted`，和`GameChatUser::RestrictionMode`。 下列範例示範可能的啟發學習法，來判斷特定圖示常數的值，指派給`iconToShow`變數的`GameChatUser`指向的變數 'chatUser' 物件。

```cpp
if (chatUser->RestrictionMode != None)
{
    if (!chatUser->IsMuted)
    {
        if (chatUser->TalkingMode != NotTalking)
        {
            iconToShow = Icon_ActiveSpeaker;
        }
        else
        {
            iconToShow = Icon_InactiveSpeaker;
        }
    }
    else
    {
        iconToShow = Icon_MutedSpeaker;
    }
}
else
{
    iconToShow = Icon_RestrictedSpeaker;
}
```

### <a name="game-chat-2"></a>遊戲交談 2

遊戲的聊天 2 具有聯合`game_chat_user_chat_indicator`用來表示播放程式的對談的目前、 即時狀態。 擷取這個值，藉由呼叫`chat_user::chat_indicator()`。 下列範例示範如何擷取的指標值`chat_user`物件指向的變數 'chatUser'，若要判斷特定圖示常值，指派給 'iconToShow' 變數：

```cpp
switch (chatUser->chat_indicator())
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

## <a name="muting"></a>靜音

## <a name="game-chat"></a>遊戲的對談

靜音或 unmuting 遊戲交談中的使用者會透過呼叫`ChatManager::MuteUserFromAllChannels()`或`ChatManager::UnMuteUIserFromAllChannels()`分別。 可以藉由檢查擷取使用者的靜音狀態`GameChatUser::IsMuted`或`GameChatUser::IsLocalUserMuted`。

## <a name="game-chat-2"></a>遊戲交談 2

`chat_user::chat_user_local::set_microphone_muted()`方法可用來切換的本機使用者的麥克風靜音的狀態。 當麥克風靜音時，將會不擷取任何從該麥克風的音訊。 如果使用者是在共用的裝置，例如 Kinect 靜音的狀態就會套用到所有使用者。

`chat_user::chat_user_local::microphone_muted()`方法可用來擷取本機使用者的麥克風靜音狀態。 這個方法只會反映在軟體，透過對呼叫的本機使用者的麥克風是否已靜音`chat_user::chat_user_local::set_microphone_muted()`; 這個方法不會反映硬體靜音，比方說，透過控制使用者的耳機上的按鈕。 沒有任何方法來擷取使用者的音訊裝置透過遊戲聊天 2 的硬體靜音狀態。

`chat_user::chat_user_local::set_remote_user_muted()`方法可用來切換靜音狀態相對於特定的本機使用者的遠端使用者。 當遠端使用者設為靜音時，本機使用者不會聽到任何聲音，或者收到來自遠端使用者的文字訊息。

## <a name="bad-reputation-auto-mute"></a>不正確的信譽自動靜音

一般而言，遠端使用者會開始 unmuted。 遊戲聊天和遊戲聊天 2 具有 「 錯誤信譽自動靜音 」 功能。 這表示，使用者將會靜音的狀態時啟動 （1） 的遠端使用者不是本機使用者，與朋友，以及 （2） 遠端使用者的信譽不佳旗標。 遊戲的對談 2 提供意見反應，當使用者由於這項功能靜音的狀態。 請參閱[使用遊戲聊天 2-不正確的信譽自動靜音](using-game-chat-2.md#bad-reputation-auto-mute)如需詳細資訊。

## <a name="privilege-and-privacy"></a>權限及隱私權

遊戲聊天和遊戲聊天 2 強制 Xbox Live 權限及隱私權限制在任何應用程式管理的通道或通訊關聯性上。 遊戲的對談 2 此外會提供診斷資訊，以判斷完全如何限制會影響 （例如音訊音訊的限制是否 uni 行或雙向） 的音訊的方向。

### <a name="game-chat"></a>遊戲的對談

遊戲的聊天公開權限及隱私權的資訊，透過`RestrictionMode`屬性。 它可以藉由檢查擷取`GameChatUser::RestrictionMode`。

### <a name="game-chat-2"></a>遊戲交談 2

當使用者第一次新增; 遊戲的對談 2 會執行特殊權限及隱私權限制查閱使用者的`chat_user::chat_indicator()`一律會傳回`game_chat_user_chat_indicator::silent`直到這項作業完成為止。 如果使用者使用的通訊會受到保護隱私權或權限限制，使用者的`chat_user::chat_indicator()`會傳回`game_chat_user_chat_indicator::platform_restricted`。 平台的通訊限制適用於語音和文字的聊天;絕對不會執行個體，其中文字聊天封鎖平台限制，但語音聊天不是，反之亦然。

`chat_user::chat_user_local::get_effective_communication_relationship()` 可用來協助您辨別使用者無法進行通訊，因為不完整的權限及隱私權作業時。 它會傳回強制執行遊戲聊天 2 的表單中的通訊關聯性`game_chat_communication_relationship_flags`原因關聯性可能不等於的形式設定的關聯性`game_chat_communication_relationship_adjuster`。 例如，如果查閱作業仍在進行中`game_chat_communication_relationship_adjuster`會`game_chat_communication_relationship_adjuster::intializing`。 此方法應該用於開發和偵錯的情況;不應該用來影響 UI (請參閱[UI](#UI))。

## <a name="cleanup"></a>Cleanup

「 原始遊戲對談的`ChatManager`是 WinRT 執行階段類別的參考計數介面。 因此，它的記憶體資源會回收時的最後一個的參考計數降至零，它會清除。 遊戲聊天 2 的 c + + API 的互動是透過單一執行個體;當應用程式不再需要透過遊戲聊天 2 的通訊，您應該呼叫`chat_manager::cleanup()`。 這可讓遊戲聊天 2 立即釋放已配置給管理通訊的所有資源。 如需遊戲聊天 2 的 WinRT API 和資源管理的詳細資訊，請參閱[使用遊戲聊天 2 WinRT 投影](using-game-chat-2-winrt.md#cleanup)。

## <a name="failure-model-and-debugging"></a>失敗模式和偵錯

原始的遊戲對談是 WinRT API;因此，它會報告透過例外狀況的錯誤。 非嚴重錯誤或警告會報告透過選擇性偵錯回呼。 遊戲聊天 2 的 c + + API 不會擲回例外狀況做為報告，所以您可以使用它輕鬆地從無例外狀況的專案如果慣用的非嚴重錯誤。 遊戲的對談 2，，會擲回例外狀況，告知有嚴重的錯誤。 這些錯誤是誤用的 API 之前初始化執行個體，或存取使用者物件，從遊戲交談執行個體移除後,，到遊戲交談執行個體中新增使用者等的結果。 這些錯誤會攔截在開發初期，並且能夠更正的修改與遊戲聊天 2 互動所使用的模式。 這類錯誤發生時，提示，當做在造成錯誤的原因會列印到偵錯工具中，才會引發例外狀況。 遊戲聊天 2 的 WinRT API 會遵循 WinRT 模式的報告透過例外狀況的錯誤。

## <a name="how-to-configure-popular-scenarios"></a>如何設定受歡迎的案例

請參閱[使用遊戲聊天 2-如何設定常用案例](using-game-chat-2.md#how-to-configure-popular-scenarios)如需如何設定受歡迎的案例，例如推播至演講、 小組和廣播樣式通訊案例的範例。

## <a name="pre-encode-and-post-decode-audio-manipulation"></a>預先編碼和後置解碼音訊的操作

藉由允許存取原始的麥克風音訊透過原始的遊戲聊天允許音訊操作`OnPreEncodeAudioBuffer`和`OnPostDecodeAudioBuffers`事件。 遊戲的對談 2 會公開透過輪詢模式這項功能。 如需詳細資訊，請參閱[即時音訊操作](real-time-audio-manipulation.md)。
