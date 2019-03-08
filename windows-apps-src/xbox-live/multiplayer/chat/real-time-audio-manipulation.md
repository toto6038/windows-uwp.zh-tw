---
title: 即時音訊操作
description: 了解如何管理及處理遊戲聊天 2 所擷取的聊天音訊。
ms.date: 05/10/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 遊戲聊天 2、 遊戲的對談、 語音通訊、 緩衝區操作、 音訊的操作
ms.localizationpriority: medium
ms.openlocfilehash: 7746080ea8a9698993a679b425f41442e4a6d943
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643893"
---
# <a name="real-time-audio-manipulation"></a>即時音訊操作

遊戲的聊天 2 讓開發人員就可以將自己插入至交談音訊管線，以檢查及操作玩家的對談的音訊資料。 這可以是適用於將有趣的音訊效果玩家的遊戲中的語音。 遊戲聊天 2 的音訊操作管線是透過可用於音訊資料輪詢的音訊資料流物件互動。 相對於使用回呼，此模型可讓開發人員檢查或操作音訊在任何處理執行緒是最方便的時候。

使用即時音訊操作的簡短逐步解說被精選如下，其中包含下列主題：

1. [初始化音訊操作管線](#initializing-the-audio-manipulation-pipeline)
2. [處理的音訊資料流狀態變更](#processing-audio-stream-state-changes)
3. [管理預先編碼聊天音訊](#manipulating-pre-encode-chat-audio-data)
4. [後續操作解碼聊天音訊](#manipulating-post-decode-chat-audio-data)
5. [交談的使用者存留期](#chat-user-lifetimes)

## <a name="initializing-the-audio-manipulation-pipeline"></a>初始化音訊操作管線

對談的遊戲 2，預設將不會啟用即時音訊的操作。 若要啟用即時音訊的操作，應用程式必須指定想要的表單上的音訊的操作中啟用`chat_manager::initialize()`audioManipulationMode 參數設定。

目前支援下列音訊操作表單：

* `game_chat_audio_manipulation_mode_flags::none` -停用音訊的操作。 這是預設設定。 在此模式中，對談音訊將傳送不中斷。
* `game_chat_audio_manipulation_mode_flags::pre_encode_stream_manipulation` -可讓預先編碼音訊的操作。 在此模式中，本機使用者所產生的所有交談音訊會都傳送音訊操作管線編碼之前。 即使應用程式是只檢查對談的音訊資料，且未對它進行操作，仍是進行提交回到遊戲聊天 2 不變的音訊緩衝區，讓它們可以編碼和傳輸的應用程式的責任。
* `game_chat_audio_manipulation_mode_flags::post_decode_stream_manipulation` -可讓後置解碼音訊的操作。 在此模式中，接收來自遠端使用者的所有交談音訊會都傳送音訊操作管線之後解碼接收者，但它呈現之前。 即使應用程式是只檢查對談的音訊資料，且未對它進行操作，仍是案例混合及提交回遊戲聊天 2 不變的音訊緩衝區，讓它們可以轉譯的應用程式的責任。

## <a name="processing-audio-stream-state-changes"></a>處理的音訊資料流狀態變更

遊戲的對談 2 提供透過音訊資料流的狀態更新`game_chat_stream_state_change`結構。 這些更新會儲存哪些資料流已更新，以及它如何已更新的資訊。 這些更新可以透過呼叫輪詢`chat_manager::start_processing_stream_state_changes()`和`chat_manager::finish_processing_stream_state_changes()`一組方法。 這一組方法會提供所有最新、 已排入佇列的音訊資料流的狀態更新為陣列`game_chat_stream_state_change`結構的指標。 應用程式應逐一查看陣列，並適當地處理每個更新。 一次所有可用`game_chat_stream_state_change`已處理的更新，該陣列應該傳遞回受遊戲對談的 2 到`chat_manager::finish_processing_stream_state_changes()`。 例如：

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            HandlePreEncodeAudioStreamCreated(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
```

## <a name="manipulating-pre-encode-chat-audio-data"></a>管理預先編碼聊天音訊資料

遊戲的聊天 2 提供存取透過本機使用者的聊天音訊資料的預先編碼`pre_encode_audio_stream`類別。

### <a name="stream-lifetime"></a>Stream 存留期
當新`pre_encode_audio_stream`執行個體可供使用應用程式使用，它會透過傳遞`game_chat_stream_state_change`結構，其具有`state_change_type`欄位設定為`game_chat_stream_state_change_type::pre_encode_audio_stream_created`。 一旦此資料流狀態變更為傳回至遊戲聊天 2，音訊資料流將會變成可預先編碼音訊的操作。

當現有`pre_encode_audio_stream`會變成音訊的操作，將無法使用時，應用程式將會收到通知透過`game_chat_stream_state_change`和它的結構`state_change_type`欄位設定為`game_chat_stream_state_change_type::pre_encode_audio_stream_closed`。 這是應用程式的機會，若要開始清除與此音訊資料流相關聯的資源。 一旦此資料流狀態變更為傳回至遊戲聊天 2，音訊資料流將變成無法使用的預先編碼音訊的操作。

當關閉`pre_encode_audio_stream`已傳回的所有資源，會終結資料流，並透過通知應用程式`game_chat_stream_state_change`和它的結構`state_change_type`欄位設定為`game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed`。 任何參考或指標，此資料流中清除。 一旦此資料流狀態變更為傳回至遊戲聊天 2，音訊資料流記憶體將會變成無效。

### <a name="stream-users"></a>Stream 使用者
可檢查資料流相關聯的使用者清單，使用`pre_encode_audio_stream::get_users()`。

### <a name="audio-formats"></a>音訊格式
可檢查應用程式會擷取從遊戲聊天 2 之緩衝區的音訊格式使用`pre_encode_audio_stream::get_pre_processed_format()`。 預先處理的音訊格式一律為 mono。 應用程式應該會處理資料表示為 32 位元浮點數、 16 位元整數和 32 位元整數。

應用程式必須通知遊戲的音訊格式的編碼和傳輸使用提交給它的操作緩衝區對談 2 `pre_encode_audio_stream::set_processed_format()`。 已處理的格式的預先編碼的音訊資料流必須符合這些前置條件：

* 格式必須是單聲道。
* 格式必須是 32 位元浮點 PCM，32 位元整數 PCM 或 16 位元整數 PCM 格式。
* 格式的採樣速率必須遵循的平台為基礎的前置條件。 Xbox One 紀元支援 8 kHz、 12 kHz、 16 kHz 和 24 kHz 範例速率。 Xbox One 和 PC 的 UWP 支援 8 kHz、 12 kHz、 16 kHz、 24 kHz、 32 kHz，44.1 kHz 和 48 kHz 範例速率。

### <a name="retrieving-and-submitting-audio"></a>擷取並送出音訊
應用程式可以查詢預先編碼的音訊串流處理使用可用的緩衝區數目`pre_encode_audio_stream::get_available_buffer_count()`。 如果應用程式想要延遲音訊處理，直到可用的最小緩衝區數目，則可以使用這項資訊。 只有 10 個緩衝區會在每個排入佇列預先編碼的音訊資料流和音訊的延遲即將推出音訊管線中的延遲，因此建議您使用應用程式清空其之前它們排入佇列 4 個以上的緩衝區，預先編碼的音訊串流。

應用程式可以擷取從音訊的緩衝區預先編碼的音訊資料流使用`pre_encode_audio_stream::get_next_buffer()`。 新的音訊緩衝區可平均來說，一次每 40 毫秒。 必須以釋放這個方法所傳回的緩衝區`pre_encode_audio_stream::return_buffer()`當他們完成正在使用。 最多 10 個已排入佇列或 unreturned 的緩衝區可以存在的任何時候預先編碼的音訊資料流。 達到此限制之後，一些未處理的緩衝區傳回之前，將會捨棄從 media player 的音訊來源擷取的新緩衝區。

應用程式可以提交其檢查和操作的緩衝區，回到遊戲聊天 2 編碼和傳輸使用`pre_encode_audio_stream::submit_buffer()`。 遊戲的對談 2 支援就地和就地音訊操作，因此緩衝區提交給`pre_encode_audio_stream::submit_buffer()`並不一定需要從擷取的同一個緩衝區`pre_encode_audio_stream::get_next_buffer()`。 隱私權/權限，這些已提交的緩衝區會根據此資料流相關聯的使用者套用。 每個為 40 毫秒，從此資料流的音訊中的下一步為 40 毫秒會進行編碼，並且傳送。 若要避免音訊營運，應持續聽到的音訊的緩衝區應該提交到這個資料流變的速率。

### <a name="stream-contexts"></a>Stream 內容
應用程式可以管理自訂指標大小的內容值在預先編碼使用的音訊資料流`pre_encode_audio_stream::set_custom_stream_context()`和`pre_encode_audio_stream::custom_stream_context()`。 這些自訂資料流內容很有幫助建立遊戲聊天 2 的音訊資料流與輔助資料之間的對應： 資料流中繼資料、 遊戲狀態等。

### <a name="example"></a>範例
以下是如何使用簡化的端對端範例預先編碼的一個音訊處理框架中的音訊資料流：

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            pre_encode_audio_stream* stream = streamStateChanges[streamStateChangeIndex]->pre_encode_audio_stream;
            stream->set_processed_audio_format(...);
            stream->set_custom_stream_context(...);
            HandlePreEncodeAudioStreamCreated(stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            HandlePreEncodeAudioStreamDestroyed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t preEncodeAudioStreamCount;
pre_encode_audio_stream_array preEncodeAudioStreams;
chat_manager::singleton_instance().get_pre_encode_audio_streams(&preEncodeAudioStreamCount, &preEncodeAudioStreams);
for (uint32_t preEncodeAudioStreamIndex = 0; preEncodeAudioStreamIndex < preEncodeAudioStreamCount; ++preEncodeAudioStreamIndex)
{
    pre_encode_audio_stream* stream = preEncodeAudioStreams[preEncodeAudioStreamIndex];
    StreamContext* context = reinterpret_cast<StreamContext*>(stream->custom_stream_context());

    game_chat_audio_format audio_format = stream->get_pre_processed_format();

    uint32_t preProcessedBufferByteCount;
    void* preProcessedBuffer;
    stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);

    while (preProcessedBuffer != nullptr)
    {
        void* processedBuffer = nullptr;
        switch (audio_format.bits_per_sample)
        {
            case 16:
            {
                assert (audio_format.sample_type == game_chat_sample_type::integer);
                processedBuffer = ManipulateChatBuffer<int16_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                break;
            }

            case 32:
            {
                switch (audio_format.sample_type)
                {
                    case game_chat_sample_type::integer:
                    {
                        processedBuffer = ManipulateChatBuffer<int32_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    case game_chat_sample_type::ieee_float:
                    {
                        processedBuffer = ManipulateChatBuffer<float>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    default:
                    {
                        assert(false);
                        break;
                    }
                }
                break;
            }

            default:
            {
                assert(false);
                break;
            }
        }
        // processedBuffer can be the same as preProcessedBuffer (in-place manipulation) or it can be a buffer of
        // memory not managed by Game Chat 2 (out-of-place manipulation).
        stream->submit_buffer(processedBuffer);
        // Only return buffers retrieved from Game Chat 2. Do not return foreign memory to return_buffer.
        stream->return_buffer(preProcessedBuffer);
        stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);
    }
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="manipulating-post-decode-chat-audio-data"></a>後續操作解碼聊天音訊資料

遊戲的聊天 2 提供後置解碼透過聊天音訊資料的存取權`post_decode_audio_source_stream`和`post_decode_audio_sink_stream`類別，好讓使用者可能唯一操控音訊從遠端使用者，對談音訊中的每個本機收件者。

### <a name="sources-and-sinks"></a>來源和接收器
不同於 pre-encode 管線中，模型的處理後置解碼音訊資料分成兩個類別：`post_decode_audio_source_stream`和`post_decode_audio_sink_stream`。 已解碼的音訊，從遠端使用者可以從擷取`post_decode_audio_source_stream`物件進行操作，以及傳送至`post_decode_audio_sink_stream`轉譯的物件。 這可讓針對遊戲聊天 2 的之間的整合後解碼音訊處理管線，並很有幫助音訊的中介軟體。

### <a name="stream-lifetime"></a>Stream 存留期
當新`post_decode_audio_source_stream`或`post_decode_audio_sink_stream`執行個體可供使用應用程式使用，它會透過傳遞`game_chat_stream_state_change`具有它的結構`state_change_type`欄位設定為`game_chat_stream_state_change_type::post_decode_audio_source_stream_created`或`game_chat_stream_state_change_type::post_decode_audio_sink_stream_created`分別。 一旦此資料流狀態變更為傳回至遊戲聊天 2，音訊資料流將成為可供後續解碼音訊的操作。

時的現有`post_decode_audio_source_stream`或`post_decode_audio_sink_stream`會變成要用於音訊的操作無法使用時，應用程式將會收到通知透過`game_chat_stream_state_change`和它的結構`state_change_type`欄位設定為`game_chat_stream_state_change_type::post_decode_audio_source_stream_closed`或`game_chat_stream_state_change_type::post_decode_audio_sink_stream`分別。 這是應用程式的機會，若要開始清除與此音訊資料流相關聯的資源。 一旦此資料流狀態變更為傳回至遊戲聊天 2，音訊資料流將變成無法使用的後置解碼音訊的操作。 來源資料流，這表示，沒有其他緩衝區會排入佇列進行操作。 對於接收資料流，這表示，提交緩衝區將不再呈現。

當關閉時`post_decode_audio_source_stream`或`post_decode_audio_sink_stream`已傳回的所有資源，會終結資料流，並透過通知應用程式`game_chat_stream_state_change`具有它的結構`state_change_type`欄位設定為`game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed`或`game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed`，分別。 任何參考或指標，此資料流中清除。 一旦此資料流狀態變更為傳回至遊戲聊天 2，音訊資料流記憶體將會變成無效。

### <a name="stream-users"></a>Stream 使用者
可檢查 post-decode 來源資料流相關聯的遠端使用者清單，使用`post_decode_audio_source_stream::get_users()`。 可檢查 post-decode 接收資料流相關聯的本機使用者清單，使用`post_decode_audio_sink_stream::get_users()`。

### <a name="audio-formats"></a>音訊格式
可檢查應用程式會擷取從遊戲聊天 2 之緩衝區的音訊格式使用`post_decode_audio_source_stream::get_pre_processed_format()`。 預先處理的音訊格式一律為 mono 的 16 位元整數的 PCM。

應用程式必須通知遊戲的音訊格式的轉譯使用提交給它的操作緩衝區對談 2 `post_decode_audio_sink_stream::set_processed_format()`。 已處理的格式的後置解碼音訊的接收資料流必須符合這些前置條件：

* 格式必須少於 64 個的通道。
* 格式必須是 16 位元整數 PCM （最佳化）、 20 位元整數 （以 24 位元容器） 的 PCM，PCM 的 24 位元整數，32 位元整數 PCM 或 32 位元浮點 PCM （16 位元整數 PCM 後的慣用格式）。 
* 格式的採樣速率必須介於 1000年到 200000 每秒樣本數。

### <a name="retrieving-and-submitting-audio"></a>擷取並送出音訊
應用程式可以查詢後解碼音訊來源資料流，可用來處理使用的緩衝區數目`post_decode_audio_source_stream::get_available_buffer_count()`。 如果應用程式想要延遲音訊處理，直到可用的最小緩衝區數目，則可以使用這項資訊。 只有 10 個緩衝區會在每個排入佇列後解碼音訊來源資料流和音訊的延遲即將推出音訊管線中的延遲，因此建議您使用應用程式清空其後將音訊資料流解碼之前它們排入佇列 4 個以上的緩衝區。

應用程式可以擷取從音訊緩衝區後解碼音訊來源資料流使用`post_decode_audio_source_stream::get_next_buffer()`。 新的音訊緩衝區可平均來說，一次每 40 毫秒。 必須以釋放這個方法所傳回的緩衝區`post_decode_audio_source_stream::return_buffer()`當他們完成正在使用。 最多 10 個已排入佇列或 unreturned 的緩衝區可存在於任何指定時間後解碼音訊來源資料流。 達到此限制之後，新的解碼的緩衝區，從遠端的播放程式皆會予以捨棄之前一些未處理的緩衝區會傳回。

應用程式可以提交其檢查和操作的緩衝區，回到受遊戲對談的 2 到後置解碼轉譯所使用的音訊的接收資料流`post_decode_audio_sink_stream::submit_mixed_buffer()`。 遊戲的對談 2 支援就地和就地音訊操作，因此緩衝區提交給`post_decode_audio_sink_stream::submit_mixed_buffer()`並不一定需要從擷取的同一個緩衝區`post_decode_audio_source_stream::get_next_buffer()`。 每個為 40 毫秒，將會呈現音訊從此資料流中的下一步為 40 毫秒。 若要避免音訊營運，應持續聽到的音訊的緩衝區應該提交到這個資料流變的速率。

### <a name="privacy-and-mixing"></a>隱私權和混用
由於 post-decode 管線的來源接收模型，必須負責應用程式的混合從擷取的緩衝區`post_decode_audio_source_stream`物件，並提交至混合式的緩衝區`post_decode_audio_sink_stream`轉譯的物件。 這也表示它是使用適當的隱私權和強制執行的權限執行的混合應用程式的責任。 遊戲的對談 2 提供`post_decode_audio_sink_stream::can_receive_audio_from_source_stream()`進行這項資訊既簡單又有效率的查詢。

### <a name="chat-indicators"></a>對談指標

後置解碼音訊操作不會影響每個使用者的聊天指標狀態。 比方說，當遠端使用者設為靜音，音訊會提供應用程式，但該遠端使用者的聊天指標仍然會指出靜音。 當遠端使用者進行通訊時，會提供其音訊，但對談指標會指出不論應用程式是否提供音訊混合包含來自該使用者的音訊交談。 如需有關 UI 和聊天指標的詳細資訊，請參閱[遊戲聊天 2 使用](using-game-chat-2.md#ui)。 如果額外的應用程式特定的限制用來判斷哪些使用者會出現在音訊的混合，是要讀取的遊戲聊天 2 所提供的對談指標時，請考慮這些相同的限制的應用程式的責任。

### <a name="stream-contexts"></a>Stream 內容
應用程式可以管理自訂指標大小的內容值在後置解碼使用的音訊資料流`set_custom_stream_context()`和`custom_stream_context()`方法。 這些自訂資料流內容很有幫助建立遊戲聊天 2 的音訊資料流與輔助資料之間的對應： 資料流中繼資料、 遊戲狀態等。

### <a name="example"></a>範例
以下是簡化的端對端範例，說明如何使用後置解碼一個音訊處理框架中的音訊資料流：

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::post_decode_audio_source_stream_created:
        {
            post_decode_audio_source_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_source_stream;
            stream->set_custom_stream_context(...);
            HandlePostDecodeAudioSourceStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_closed:
        {
            HandlePostDecodeAudioSourceStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed:
        {
            HandlePostDecodeAudioSourceStreamDestroyed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_created:
        {
            post_decode_audio_sink_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_sink_stream;
            stream->set_custom_stream_context(...);
            stream->set_processed_format(...);
            HandlePostDecodeAudioSinkStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_closed:
        {
            HandlePostDecodeAudioSinkStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed:
        {
            HandlePostDecodeAudioSinkStreamDestroyed(stream);
            break;
        }

        ...
    }
}

chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t sourceStreamCount;
post_decode_audio_source_stream_array sourceStreams;
chatManager::singleton_instance().get_post_decode_audio_source_streams(&sourceStreamCount, &sourceStreams);

uint32_t sinkStreamCount;
post_decode_audio_sink_stream_array sinkStreams;
chatManager::singleton_instance().get_post_decode_audio_sink_streams(&sinkStreamCount, &sinkStreams);

//
// MixBuffer is a custom type defined as:
// struct MixBuffer
// {
//     uint32_t bufferByteCount;
//     void* buffer;
// };
//
std::vector<std::pair<post_decode_audio_source_stream*, MixBuffer>> cachedSourceBuffers;

for (uint32_t sourceStreamIndex = 0; sourceStreamIndex < sourceStreamCount; ++sourceStreamIndex)
{
    post_decode_audio_source_stream* sourceStream = sourceStreams[sourceStreamIndex];

    MixBuffer mixBuffer;
    sourceStream->get_next_buffer(&mixBuffer.bufferByteCount, &mixBuffer.buffer);
    if (buffer != nullptr)
    {
        // Stash the buffer to return after we are done with mixing. If this program was using audio middleware, now
        // would be an appropriate time to plumb the buffer through the middleware
        cachedSourceBuffer.push_back(std::pair<post_decode_audio_source_stream*, MixBuffer>{sourceStream, mixBuffer});
    }
}

// Loop over each sink stream, perform mixing, and submit
for (uint32_t sinkStreamIndex = 0; sinkStreamIndex < sinkStreamCount; ++sinkStreamIndex)
{
    post_decode_audio_sink_stream* sinkStream = sinkStreams[sinkStreamIndex];

    if (sinkStream->is_open())
    {
        std::vector<std::pair<MixBuffer, float>> buffersToMixForThisStream;

        for (const std::pair<post_decode_audio_source_stream, MixBuffer>& sourceBufferPair : cachedSourceBuffers)
        {
            float volume;
            if (sinkStream->can_receive_audio_from_source_stream(sourceBufferPair.first, &volume))
            {
                buffersToMixForThisStream.push_back(std::pair<MixBuffer, float>{sourceBufferPair.second, volume});
            }
        }

        if (buffersToMixForThisStream.size() > 0)
        {
            uint32_t mixedBufferByteCount;
            uint8_t* mixedBuffer;
            MixPostDecodeBuffers(buffersToMixForThisStream, &mixedBufferByteCount, &mixedBuffer);
            sinkStream->submit_mixed_buffer(mixedBufferByteCount, mixedBuffer);
        }
    }
}

// Return buffers after mix and submission
for (const std::pair<post_decode_audio_source_stream*, MixBuffer>& cachedSourceBuffer : cachedSourceBuffers)
{
    post_decode_audio_source_stream* sourceStream = cachedSourceBuffer.first;
    void* bufferToReturn = cachedSourceBuffer.second.buffer;
    sourceStream->return_buffer(bufferToReturn);
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="chat-user-lifetimes"></a>交談的使用者存留期

啟用即時音訊的操作，將會影響交談的使用者的存留期。 如果`chat_manager::remove_user(chatUserX)`會呼叫 chat_user chatUserX 所指向的物件會維持有效，直到所有參考 chatUserX 的音訊資料流皆已終結。 請考慮下列案例：

```cpp
// At somepoint a chat user, chatUserX, leaves the game session.
chat_manager::singleton_instance().remove_user(chatUserX);

// chatUserX is still valid, but to avoid further synchronization, prevent non-audio-stream use of chatUserX
chatUserX = nullptr;

// On the audio processing thread...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        // All of the streams associated with chatUserX will close.
        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            CleanupPreEncodeAudioStreamResources(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

// The next time the app processes stream state changes...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            uint32_t chatUserCount;
            Xs::game_chat_2::chat_user_array chatUsers;
            streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream->get_users(&chatUserCount, &chatUsers);
            assert(chatUserCount != 0);
            for (uint32_t chatUserIndex = 0; chatUserIndex < chatUserCount; ++chatUserIndex)
            {
                // chat_user objects such as chatUserX will still be valid while the destroyed state change is being processed.
                Log(chatUsers[chatUserIndex]->xbox_user_id());
            }
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
// Once the all destroyed state changes have been processed for all streams associated with chatUserX, it's memory will be invalidated.
// Do not call methods on chatUserX, e.g. chatUserX->xbox_user_id()
```
